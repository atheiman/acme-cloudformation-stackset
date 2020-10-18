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
```
