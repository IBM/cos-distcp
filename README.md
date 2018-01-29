# COSDistCp
COSDistCp is a tool based on the Community DistCp that has been adapted to work with IBM Cloud Object Storage (COS). 

## Build

	git clone https://github.com/IBM/cos-distcp.git
	cd cos-distcp
	mvn clean package
	

## Usage
COSDistCp needs a connector to access IBM Cloud Object Storage. COSDistCp can be used with  [Stocator](https://github.com/SparkTC/stocator) that supports HMAC and IAM authentication models or community Hadoop AWS connector that supports HMAC only.
### Usage with Stocator

#### Building Stocator with IAM authentication support 
Stocator with IAM support is not yet pushed into master branch. To enable Stocator with IAM there is need to build Stocator locally

	git clone https://github.com/SparkTC/stocator.git
	cd stocator
	git checkout 1.0.14-SNAPSHOT-ibm-sdk
	mvn clean -DskipTests package

Follow [Getting IAM authentication keys](auth/README.md) if COS is used with IAM athentication

#### Copy from HDFS to COS (IAM authentication):
	
	hadoop jar <path-to:cos-distcp>/target/cosdistcp-0.8-SNAPSHOT-jar-with-dependencies.jar com.ibm.cosdistcp.COSDistCp -D fs.stocator.scheme.list=cos -D fs.cos.impl=com.ibm.stocator.fs.ObjectStoreFileSystem -D fs.stocator.cos.impl=com.ibm.stocator.fs.cos.COSAPIClient -D fs.stocator.cos.scheme=cos -D fs.cos.<SERVICE_NAME>.endpoint=<endpoint> -D fs.cos.<SERVICE_NAME>.iam.service.id=<ServiceId-...> -D fs.cos.<SERVICE_NAME>.iam.api.key=<apikey> -libjars <path-to:stocator>/target/stocator-1.0.14-IBM-SDK.jar <in_dir> cos://<BUCKET_NAME.SERVICE_NAME/out_dir>

#### Copy from OpenStack Swift API object storage to COS (IAM authentication):
	hadoop jar <path-to:cos-distcp>/target/cosdistcp-0.8-SNAPSHOT-jar-with-dependencies.jar com.ibm.cosdistcp.COSDistCp -D fs.stocator.scheme.list=cos -D fs.cos.impl=com.ibm.stocator.fs.ObjectStoreFileSystem -D fs.stocator.cos.impl=com.ibm.stocator.fs.cos.COSAPIClient -D fs.stocator.cos.scheme=cos -D fs.cos.<SERVICE_NAME_1>.endpoint=<endpoint> -D fs.cos.<SERVICE_NAME_1>.iam.service.id=<ServiceId-...> -D fs.cos.<SERVICE_NAME_1>.iam.api.key=<apikey> -D fs.swift2d.service.<SERVICE_NAME_2>.auth.url=https://identity.open.softlayer.com/v3/auth/tokens -D fs.swift2d.service.<SERVICE_NAME_2>.tenant=<PROJECTID> -D fs.swift2d.service.<SERVICE_NAME_2>.username=<USERID> -D fs.swift2d.service.<SERVICE_NAME_2>.password=<PASSWORD> -D fs.swift2d.service.<SERVICE_NAME_2>.region=dallas -D fs.swift2d.service.<SERVICE_NAME_2>.public=true -libjars <path-to:stocator>/target/stocator-1.0.14-IBM-SDK.jar,<path-to:simple-json-jar>/json-simple-1.1.jar swift2d://<BUCKET_NAME.SERVICE_NAME_2>/in_dir> cos://<BUCKET_NAME.SERVICE_NAME_1/out_dir>

#### Copy from COS (IAM) to COS (IAM):
	hadoop jar <path-to:cos-distcp>/target/cosdistcp-0.8-SNAPSHOT-jar-with-dependencies.jar com.ibm.cosdistcp.COSDistCp -D fs.stocator.scheme.list=cos -D fs.cos.impl=com.ibm.stocator.fs.ObjectStoreFileSystem -D fs.stocator.cos.impl=com.ibm.stocator.fs.cos.COSAPIClient -D fs.stocator.cos.scheme=cos -D fs.cos.<SERVICE_NAME_1>.endpoint=<endpoint> -D fs.cos.<SERVICE_NAME_1>.iam.service.id=<ServiceId-...> -D fs.cos.<SERVICE_NAME_1>.iam.api.key=<apikey> -D fs.cos.<SERVICE_NAME_2>.endpoint=<endpoint> -D fs.cos.<SERVICE_NAME_2>.iam.service.id=<ServiceId-...> -D fs.cos.<SERVICE_NAME_2>.iam.api.key=<apikey> -libjars <path-to:stocator>/target/stocator-1.0.14-IBM-SDK.jar,<path-to:simple-json-jar>/json-simple-1.1.jar cos://<BUCKET_NAME1.SERVICE_NAME_1>/in_dir> cos://<BUCKET_NAME2.SERVICE_NAME_2/out_dir>

#### Copy from COS (HMAC using access key/secret key) to COS (IAM):
	hadoop jar <path-to:cos-distcp>/target/cosdistcp-0.8-SNAPSHOT-jar-with-dependencies.jar com.ibm.cosdistcp.COSDistCp -D fs.stocator.scheme.list=cos -D fs.cos.impl=com.ibm.stocator.fs.ObjectStoreFileSystem -D fs.stocator.cos.impl=com.ibm.stocator.fs.cos.COSAPIClient -D fs.stocator.cos.scheme=cos -D fs.cos.<SERVICE_NAME_1>.endpoint=<endpoint> -D fs.cos.<SERVICE_NAME_1>.access.key=<access> -D fs.cos.<SERVICE_NAME_1>secret.key=<secret> -D fs.cos.<SERVICE_NAME_2>.endpoint=<endpoint> -D fs.cos.<SERVICE_NAME_2>.iam.service.id=<ServiceId-...> -D fs.cos.<SERVICE_NAME_2>.iam.api.key=<apikey> -libjars <path-to:stocator>/target/stocator-1.0.14-IBM-SDK.jar,<path-to:simple-json-jar>/json-simple-1.1.jar cos://<BUCKET_NAME1.SERVICE_NAME_1>/in_dir> cos://<BUCKET_NAME2.SERVICE_NAME_2/out_dir>

#### Copy from HDFS to COS (HMAC using access key/secret key):
	hadoop jar path-to:cos-distcp-0.8-SNAPSHOT-jar-with-dependencies.jar com.ibm.cosdistcp.COSDistCp -D fs.stocator.scheme.list=cos -D fs.cos.impl=com.ibm.stocator.fs.ObjectStoreFileSystem -D fs.stocator.cos.impl=com.ibm.stocator.fs.cos.COSAPIClient -D fs.stocator.cos.scheme=cos -libjars path-to-stocator-1.0.14-SNAPSHOT-jar-with-dependencies.jar <in_dir> cos://<bucket>.<service>/<out_dir>

### Usage with Hadoop s3a connector
#### Copy from HDFS to COS (HMAC using access key/secret key):
	hadoop jar path-to:cos-distcp/target/cosdistcp-0.8-SNAPSHOT-jar-with-dependencies.jar com.ibm.cosdistcp.COSDistCp -libjars path-to:hadoop-aws.jar <in_dir> s3a://<bucket>/<out_dir>

