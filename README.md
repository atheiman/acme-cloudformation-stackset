# Acme::CloudFormation::StackSet

This repo serves as an example of how to deploy [the CloudFormation resource `AWS::CloudFormation::StackSet`](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-cloudformation-stackset.html) to a GovCloud account, where the resource is not yet available. This is possible because the code for the `AWS::CloudFormation::StackSet` resource is open source, available at https://github.com/aws-cloudformation/aws-cloudformation-resource-providers-cloudformation/tree/master/acme-cloudformation-stackset.

## Notes

1. Change all instances of `AWS::CloudFormation::StackSet` to `Acme::CloudFormation::StackSet`
1. Change all instances of `aws-cloudformation-stackset` to `acme-cloudformation-stackset`
1. Rename all files named `*aws-cloudformation-stackset*` to `*acme-cloudformation-stackset*`

```sh
brew install maven

brew tap adoptopenjdk/openjdk
brew cask install adoptopenjdk8

mvn --version
# Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
# Maven home: /usr/local/Cellar/maven/3.6.3_1/libexec
# Java version: 1.8.0_265, vendor: AdoptOpenJDK, runtime: /Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home/jre
# Default locale: en_US, platform encoding: UTF-8
# OS name: "mac os x", version: "10.15.7", arch: "x86_64", family: "mac"

java -version
# openjdk version "1.8.0_265"
# OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_265-b01)
# OpenJDK 64-Bit Server VM (AdoptOpenJDK)(build 25.265-b01, mixed mode)ll

echo $JAVA_HOME
# /Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home

pip install cloudformation-cli cloudformation-cli-java-plugin cloudformation-cli-go-plugin cloudformation-cli-python-plugin
cfn --version
# cfn 0.1.11

mvn package
# [INFO] Scanning for projects...
# [INFO]
# [INFO] --< software.amazon.cloudformation.stackset:acme-cloudformation-stackset-handler >--
# [INFO] Building acme-cloudformation-stackset-handler 1.0-SNAPSHOT
# [INFO] --------------------------------[ jar ]---------------------------------
# [INFO]
#
# ...
#
# [INFO] ------------------------------------------------------------------------
# [INFO] BUILD SUCCESS
# [INFO] ------------------------------------------------------------------------
# [INFO] Total time:  20.396 s
# [INFO] Finished at: 2020-10-18T17:18:48-05:00
# [INFO] ------------------------------------------------------------------------

cfn submit --dry-run
# Dry run complete: /Users/austinheiman/code/github.com/atheiman/acme-cloudformation-stackset/acme-cloudformation-stackset.zip

cfn submit --region us-gov-west-1 --endpoint-url https://cloudformation.us-gov-west-1.amazonaws.com --set-default --verbose
# Validating your resource schema...
# Creating acme-cloudformation-stackset-role-stack
# acme-cloudformation-stackset-role-stack stack was successfully created
# Creating CloudFormationManagedUploadInfrastructure
# ...
# Successfully submitted type. Waiting for registration with token '<redacted>' to complete.
# Registration complete.
# ...
# Set default version to 'arn:aws-us-gov:cloudformation:us-gov-west-1:111111111111:type/resource/Acme-CloudFormation-StackSet/00000001

aws cloudformation list-types --region us-gov-west-1 --visibility PRIVATE
# {
#     "TypeSummaries": [
#         {
#             "Type": "RESOURCE",
#             "TypeName": "Acme::CloudFormation::StackSet",
#             "DefaultVersionId": "00000001",
#             "TypeArn": "arn:aws-us-gov:cloudformation:us-gov-west-1:111111111111:type/resource/Acme-CloudFormation-StackSet",
#             "LastUpdated": "2020-10-18T22:30:33.629000+00:00",
#             "Description": "StackSet as a resource provides one-click experience for provisioning a StackSet and StackInstances"
#         }
#     ]
# }
```

### cfn cli cloudformation-cli-java-plugin codegen.py errors

At one point I had issues with the cfn cli (`cloudformation-cli-java-plugin==2.0.2`) not finding my built jar. The issue was surfaced in `/Users/austinheiman/.pyenv/versions/3.8.1/lib/python3.8/site-packages/rpdk/java/codegen.py`. In the `_find_jar(project)` method, I commented out [the `if not jar_glob:` and `if len(jar_glob) > 1:` blocks](https://github.com/aws-cloudformation/cloudformation-cli-java-plugin/blob/v2.0.2/python/rpdk/java/codegen.py#L435-L448), and that solved the issue. I did some light debugging, and the method _was_ finding the jar by glob search, so not sure why it was raising errors there.

After going thru this process once, a few days later I reset that `codegen.py` file and had no issues with the `_find_jar(project)` method the second time.
