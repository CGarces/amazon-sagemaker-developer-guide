# Edge Manager Agent<a name="edge-device-fleet-about"></a>

The Edge Manager agent is an inference engine for your edge devices\. Use the agent to make predictions with models loaded onto your edge devices\. The agent also collects model metrics and captures data at specific intervals\. Sample data is stored in your Amazon S3 bucket\.

## How the Agent Works<a name="edge-device-fleet-how-agent-works"></a>

The agent runs on the CPU of your devices\. The agent will run inference on the framework and hardware of the target device you specified during the compilation job\. For example, if you compiled your model for the Jetson Nano, the agent will support the GPU in the provided [Deep Learning Runtime](https://github.com/neo-ai/neo-ai-dlr) \(DLR\)\.

The agent is released in binary format for supported operating systems\. Check your operating system is supported and meets the minimum OS requirement in the following table:

------
#### [ Linux ]

**Version:** Ubuntu 18\.04

**Supported Binary Formats:** x86\-64 bit \(ELF binary\) and ARMv8 64 bit \(ELF binary\)

------
#### [ Windows ]

**Version:** Windows 10 version 1909

**Supported Binary Formats:** x86\-32 bit \(DLL\) and x86\-64 bit \(DLL\)

------

## Installing Edge Manager agent<a name="edge-device-fleet-installation"></a>

To use Edge Manager agent, you first need to obtain the release artifacts and a Root Certificate\. The release artifacts are stored in an Amazon S3 bucket in the `us-west-2` Region\. To download the artifacts, specify your operating system \(`<OS>`\) and the `VERSION`\.

Based on your operating system, replace `<OS>` with one of the following:


| Windows 32\-bit | Windows 64\-bit | Linux x86\-64 | Linux ARMv8 | 
| --- | --- | --- | --- | 
| windows\-x86 | windows\-x64 | linux\-x64 | linux\-armv8 | 

The `VERSION` is broken into three components: `<MAJOR_VERSION>.<YYYY-MM-DD>-<SHA-7>`, where:
+ `MAJOR_VERSION`: The release version\. The release version is currently set to `1`\.
+ `<YYYY-MM-DD>`: The time stamp of the artifacts release\.
+ `SHA-7`: The repository commit ID from which the release is built\.

You need to provide the `MAJOR_VERSION` and the time stamp in`<YYYY-MM-DD>` format\. We suggest you use the latest artifact release time stamp\. Use the following to get the latest time stamp\.

Run the following in your command line to get the latest time stamp\. Replace `<OS>` with your operating system:

```
aws s3 ls s3://sagemaker-edge-release-store-us-west-2-<OS>/Releases/ | sort -r
```

For example, if you have a Windows 32\-bit OS, run:

```
aws s3 ls s3://sagemaker-edge-release-store-us-west-2-windows-x86/Releases/ | sort -r
```

This returns:

```
2020-12-01 23:33:36 0 

                    PRE 1.20201218.81f481f/
                    PRE 1.20201207.02d0e97/
```

The return output in this example shows two release artifacts\. The first release artifact file notes that the release version: has a major release version of `1`, a time stamp of `20201218` \(in <YYYY\-MM\-DD> format\), and a `81f481f` SHA\-7 commit ID\.

**Note**  
The above command assumes you have configured AWS Command Line Interface\. For more information, about how to configure the settings that the AWS CLI uses to interact with AWS, see [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)\.

Based on your operating system, use the following commands to install the artifacts:

------
#### [ Windows 32\-bit ]

```
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-windows-x86/Releases/<VERSION>/<VERSION>.zip .
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-windows-x86/Releases/<VERSION>/sha256_hex.shasum .
```

------
#### [ Windows 64\-bit ]

```
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-windows-x64/Releases/<VERSION>/<VERSION>.zip .
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-windows-x64/Releases/<VERSION>/sha256_hex.shasum .
```

------
#### [ Linux x86\-64 ]

```
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-linux-x64/Releases/<VERSION>/<VERSION>.tgz .
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-linux-x64/Releases/<VERSION>/sha256_hex.shasum .
```

------
#### [ Linux ARMv8 ]

```
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-linux-armv8/Releases/<VERSION>/<VERSION>.tgz .
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-linux-armv8/Releases/<VERSION>/sha256_hex.shasum .
```

------

You will also need to download a Root Certificate\. This certificate validates model artifacts signed by AWS before loading them onto your edge devices\.

Replace `<OS>` corresponding to your platform from the list of supported operation systems and replace `<REGION>` with your AWS region\.

```
aws s3 cp s3://sagemaker-edge-release-store-us-west-2-<OS>/Certificates/<REGION>/<REGION>.pem .
```

### Running SageMaker Edge Manager agent<a name="edge-device-fleet-running-agent"></a>

SageMaker Edge Manager agent can be run as a standalone process in the form of an Executable and Linkable Format \(ELF\) executable binary or can be linked against as a Dynamic Shared Object \(\.dll\)\. Running as a standalone executable binary is the preferred mode and is supported on Linux\. Running as a shared object \(\.dll\) is supported on Windows\.

On Linux, we recommend that you run the binary via a service that’s a part of your initialization \(init\) system\. If you want to run the binary directly, you can do so in a terminal as shown below\. If you have a modern OS, there are no other installations necessary prior to running the agent, since all the requirements are statically built into the executable\. This gives you flexibility to run the agent on the terminal, as a service, or within a container\.

```
./sagemaker_edge_agent -a <ADDRESS_TO_SOCKET> -c <PATH_TO_CONFIG_FILE>
```

### Obtain AWS IoT Credentials Certificates<a name="edge-device-fleet-agent-certificates"></a>

SageMaker Edge Manager takes advantage of the AWS IoT Core services to facilitate the connection between the edge devices and endpoints in the AWS cloud\. By doing so, you can take advantage of existing AWS IoT functionality after you set up your devices to work with Edge Manager\.

To connect your device to AWS IoT, you need to create AWS IoT thing objects, create and register a client certificate with AWS IoT, and create and configure an IAM role for your devices\.

Follow the instructions from the [How to Eliminate the Need for Hardcoded AWS Credentials in Devices by Using the AWS IoT Credentials Provider](https://aws.amazon.com/blogs/security/how-to-eliminate-the-need-for-hardcoded-aws-credentials-in-devices-by-using-the-aws-iot-credentials-provider/) blog post for detailed instructions on how to create and register a client certificate with AWS IoT, and create and configure an IAM role for your devices\. For general information on authorizing AWS services, see [Authorizing direct calls to AWS services](https://docs.aws.amazon.com/iot/latest/developerguide/authorizing-direct-aws.html)\. 

### Models on Edge Devices<a name="edge-device-fleet-agent-models"></a>

The Edge Manager agent can load one model at a time and make inference with that model on edge devices\. The agent validates the model signature and loads into memory all the artifacts produced by the edge packaging job\. This step requires all the required certificates described in previous steps to be installed along with rest of the binary installation\. If the model’s signature cannot be validated, then loading of the model fails with appropriate return code and reason\.

See [Installing Edge Manager agent](#edge-device-fleet-installation) for instructions on how to download the root certificate and binary release artifacts\. For information about creating and registering a client certificate with AWS IoT and creating and configuring an IAM role for your devices see [Security in AWS IoT](https://docs.aws.amazon.com/iot/latest/developerguide/security.html)\.