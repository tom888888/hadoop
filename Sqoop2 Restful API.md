### Sqoop2 Restful API

1. **/version -  [GET] - Get Sqoop Version**

   ```
   http://192.168.56.100:12000/sqoop/version
   ```

   Response

   ```json
   {
       "source-revision": "435d5e61b922a32d7bce567fe5fb1a9c0d9b1bbb",
       "source-url": "git://mbp.abrahamfine.com/Users/abefine/cloudera_code/sqoop-clean/common",
       "build-date": "Tue Jul 19 16:08:27 PDT 2016",
       "user": "abefine",
       "api-versions": [
           "v1"
       ],
       "build-version": "1.99.7"
   }
   ```

2. Get All Connectors

3. **/v1/connector/[cname] - [GET] - Get Connectors**

   ```
   http://192.168.56.100:12000/sqoop/v1/connector/hdfs-connector
   ```

   Response

   ```json
   {
       "connectors": [
           {
               "job-config": {
                   "FROM": {
                       "configs": [
                           {
                               "validators": [],
                               "inputs": [
                                   {
                                       "size": 255,
                                       "editable": "ANY",
                                       "validators": [],
                                       "name": "fromJobConfig.inputDirectory",
                                       "id": 68,
                                       "sensitive": false,
                                       "overrides": "",
                                       "type": "STRING"
                                   },
                                   {
                                       "editable": "ANY",
                                       "validators": [],
                                       "name": "fromJobConfig.overrideNullValue",
                                       "id": 69,
                                       "sensitive": false,
                                       "overrides": "",
                                       "type": "BOOLEAN"
                                   },
                                   {
                                       "size": 255,
                                       "editable": "ANY",
                                       "validators": [],
                                       "name": "fromJobConfig.nullValue",
                                       "id": 70,
                                       "sensitive": false,
                                       "overrides": "",
                                       "type": "STRING"
                                   }
                               ],
                               "name": "fromJobConfig",
                               "id": 15,
                               "type": "JOB"
                           },
                           {
                               "validators": [],
                               "inputs": [
                                   {
                                       "editable": "ANY",
                                       "validators": [],
                                       "values": "NONE,NEW_FILES",
                                       "name": "incremental.incrementalType",
                                       "id": 71,
                                       "sensitive": false,
                                       "overrides": "",
                                       "type": "ENUM"
                                   },
                                   {
                                       "editable": "ANY",
                                       "validators": [],
                                       "name": "incremental.lastImportedDate",
                                       "id": 72,
                                       "sensitive": false,
                                       "overrides": "",
                                       "type": "DATETIME"
                                   }
                               ],
                               "name": "incremental",
                               "id": 16,
                               "type": "JOB"
                           }
                       ],
                       "validators": []
                   },
                   "TO": {
                       "configs": [
                           {
                               "validators": [],
                               "inputs": [
                                   {
                                       "editable": "ANY",
                                       "validators": [],
                                       "name": "toJobConfig.overrideNullValue",
                                       "id": 73,
                                       "sensitive": false,
                                       "overrides": "",
                                       "type": "BOOLEAN"
                                   },
                                   {
                                       "size": 255,
                                       "editable": "ANY",
                                       "validators": [],
                                       "name": "toJobConfig.nullValue",
                                       "id": 74,
                                       "sensitive": false,
                                       "overrides": "",
                                       "type": "STRING"
                                   },
                                   {
                                       "editable": "ANY",
                                       "validators": [],
                                       "values": "TEXT_FILE,SEQUENCE_FILE,PARQUET_FILE",
                                       "name": "toJobConfig.outputFormat",
                                       "id": 75,
                                       "sensitive": false,
                                       "overrides": "",
                                       "type": "ENUM"
                                   },
                                   {
                                       "editable": "ANY",
                                       "validators": [],
                                       "values": "NONE,DEFAULT,DEFLATE,GZIP,BZIP2,LZO,LZ4,SNAPPY,CUSTOM",
                                       "name": "toJobConfig.compression",
                                       "id": 76,
                                       "sensitive": false,
                                       "overrides": "",
                                       "type": "ENUM"
                                   },
                                   {
                                       "size": 255,
                                       "editable": "ANY",
                                       "validators": [],
                                       "name": "toJobConfig.customCompression",
                                       "id": 77,
                                       "sensitive": false,
                                       "overrides": "",
                                       "type": "STRING"
                                   },
                                   {
                                       "size": 255,
                                       "editable": "ANY",
                                       "validators": [],
                                       "name": "toJobConfig.outputDirectory",
                                       "id": 78,
                                       "sensitive": false,
                                       "overrides": "",
                                       "type": "STRING"
                                   },
                                   {
                                       "editable": "ANY",
                                       "validators": [],
                                       "name": "toJobConfig.appendMode",
                                       "id": 79,
                                       "sensitive": false,
                                       "overrides": "",
                                       "type": "BOOLEAN"
                                   }
                               ],
                               "name": "toJobConfig",
                               "id": 17,
                               "type": "JOB"
                           }
                       ],
                       "validators": []
                   }
               },
               "name": "hdfs-connector",
               "all-config-resources": {
                   "incremental.incrementalType.label": "Incremental type",
                   "incremental.lastImportedDate.example": "1987-02-02 13:15:27",
                   "linkConfig.uri.help": "Namenode URI for your cluster.",
                   "incremental.incrementalType.help": "Type of incremental import.",
                   "fromJobConfig.nullValue.label": "Null value",
                   "linkConfig.uri.example": "hdfs://nn1.example.com/",
                   "incremental.help": "Information relevant for incremental reading from HDFS.",
                   "toJobConfig.overrideNullValue.example": "true",
                   "fromJobConfig.overrideNullValue.label": "Override null value",
                   "toJobConfig.appendMode.help": "If set to false, job will fail if output directory already exists. If set to true then imported data will be stored to already existing and possibly non empty directory.",
                   "linkConfig.confDir.help": "Directory on Sqoop server machine with hdfs configuration files (hdfs-site.xml, ...). This connector will load all files ending with -site.xml.",
                   "toJobConfig.outputDirectory.example": "/user/jarcec/output-dir",
                   "fromJobConfig.nullValue.help": "For file formats that doesn't have native representation of NULL (as for example text file) use this particular string to decode NULL value.",
                   "toJobConfig.overrideNullValue.help": "If set to true, then the null value will be overridden with the value set in Null value.",
                   "toJobConfig.nullValue.label": "Null value",
                   "toJobConfig.customCompression.example": "org.apache.hadoop.io.compress.SnappyCodec",
                   "incremental.lastImportedDate.help": "Datetime stamp of last read file. Next job execution will read only files that have been created after this point in time.",
                   "toJobConfig.outputFormat.example": "PARQUET_FILE",
                   "fromJobConfig.help": "Specifies information required to get data from HDFS.",
                   "toJobConfig.customCompression.label": "Custom codec",
                   "linkConfig.confDir.example": "/etc/hadoop/conf/",
                   "fromJobConfig.overrideNullValue.example": "true",
                   "linkConfig.confDir.label": "Conf directory",
                   "toJobConfig.label": "Target configuration",
                   "toJobConfig.help": "Configuration describing where and how the resulting data should be stored.",
                   "toJobConfig.compression.help": "Compression codec that should be use to compress transferred data.",
                   "toJobConfig.outputDirectory.help": "HDFS directory where transferred data will be written to.",
                   "connector.name": "HDFS Connector",
                   "linkConfig.configOverrides.help": "Additional configuration that will be set on HDFS Configuration object, possibly overriding any keys loaded from configuration files.",
                   "linkConfig.uri.label": "URI",
                   "fromJobConfig.inputDirectory.label": "Input directory",
                   "toJobConfig.appendMode.example": "true",
                   "toJobConfig.compression.example": "SNAPPY",
                   "linkConfig.help": "Contains configuration required to connect to your HDFS cluster.",
                   "incremental.label": "Incremental import",
                   "linkConfig.label": "HDFS cluster",
                   "incremental.incrementalType.example": "NEW_FILES",
                   "toJobConfig.overrideNullValue.label": "Override null value",
                   "fromJobConfig.inputDirectory.example": "/user/jarcec/input-dir",
                   "incremental.lastImportedDate.label": "Last imported date",
                   "toJobConfig.compression.label": "Compression codec",
                   "fromJobConfig.overrideNullValue.help": "If set to true, then the null value will be overridden with the value set in Null value.",
                   "toJobConfig.nullValue.example": "N",
                   "fromJobConfig.nullValue.example": "N",
                   "linkConfig.configOverrides.label": "Additional configs:",
                   "fromJobConfig.label": "Input configuration",
                   "toJobConfig.nullValue.help": "For file formats that doesn't have native representation of NULL (as for example text file) use this particular string to encode NULL value.",
                   "linkConfig.configOverrides.example": "custom.key=custom.value",
                   "fromJobConfig.inputDirectory.help": "Input directory containing files that should be transferred.",
                   "toJobConfig.outputFormat.help": "File format that should be used for transferred data.",
                   "toJobConfig.customCompression.help": "Fully qualified class name with Hadoop codec implementation that should be used if none of the build-in options are suitable.",
                   "toJobConfig.outputDirectory.label": "Output directory",
                   "toJobConfig.appendMode.label": "Append mode",
                   "toJobConfig.outputFormat.label": "File format"
               },
               "id": 5,
               "class": "org.apache.sqoop.connector.hdfs.HdfsConnector",
               "version": "1.99.7",
               "link-config": {
                   "configs": [
                       {
                           "validators": [],
                           "inputs": [
                               {
                                   "size": 255,
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.uri",
                                   "id": 65,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "STRING"
                               },
                               {
                                   "size": 255,
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.confDir",
                                   "id": 66,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "STRING"
                               },
                               {
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.configOverrides",
                                   "id": 67,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "MAP",
                                   "sensitive-pattern": ""
                               }
                           ],
                           "name": "linkConfig",
                           "id": 14,
                           "type": "LINK"
                       }
                   ],
                   "validators": []
               }
           }
       ]
   }
   ```

   

4. **/v1/driver - [GET] - Get Sqoop Driver**

   ```
   http://192.168.56.100:12000/sqoop/v1/driver
   ```

   Responses

   ```json
   {
       "job-config": {
           "configs": [
               {
                   "validators": [],
                   "inputs": [
                       {
                           "editable": "ANY",
                           "validators": [],
                           "name": "throttlingConfig.numExtractors",
                           "id": 88,
                           "sensitive": false,
                           "overrides": "",
                           "type": "INTEGER"
                       },
                       {
                           "editable": "ANY",
                           "validators": [],
                           "name": "throttlingConfig.numLoaders",
                           "id": 89,
                           "sensitive": false,
                           "overrides": "",
                           "type": "INTEGER"
                       }
                   ],
                   "name": "throttlingConfig",
                   "id": 22,
                   "type": "JOB"
               },
               {
                   "validators": [],
                   "inputs": [
                       {
                           "editable": "ANY",
                           "validators": [],
                           "name": "jarConfig.extraJars",
                           "id": 90,
                           "sensitive": false,
                           "overrides": "",
                           "type": "LIST"
                       }
                   ],
                   "name": "jarConfig",
                   "id": 23,
                   "type": "JOB"
               }
           ],
           "validators": []
       },
       "all-config-resources": {
           "jarConfig.help": "Classpath configuration specific to the driver",
           "throttlingConfig.numLoaders.label": "Loaders",
           "jarConfig.label": "Classpath configuration",
           "throttlingConfig.numExtractors.help": "Number of extractors that Sqoop will use",
           "throttlingConfig.numLoaders.help": "Number of loaders that Sqoop will use",
           "jarConfig.extraJars.help": "A list of the FQDNs of additional jars that are needed to execute the job",
           "throttlingConfig.numExtractors.label": "Extractors",
           "jarConfig.extraJars.label": "Extra mapper jars",
           "throttlingConfig.label": "Throttling resources",
           "throttlingConfig.help": "Set throttling boundaries to not overload your systems"
       },
       "id": 8,
       "version": "1"
   }
   ```

   

5. /v1/links

6. v1/links?cname=[cname]

7. **/v1/link/[lname] - [GET] - Get Link**

   ```
   http://192.168.56.100:12000/sqoop/v1/link/mysql
   ```

   Responses:

   ```json
   {
       "links": [
           {
               "link-config-values": {
                   "configs": [
                       {
                           "validators": [],
                           "inputs": [
                               {
                                   "size": 128,
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.jdbcDriver",
                                   "id": 1,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "STRING",
                                   "value": "com.mysql.jdbc.Driver"
                               },
                               {
                                   "size": 2000,
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.connectionString",
                                   "id": 2,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "STRING",
                                   "value": "jdbc%3Amysql%3A%2F%2F192.168.56.102%3A3306%2Fhidb%3FuseSSL%3Dfalse"
                               },
                               {
                                   "size": 40,
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.username",
                                   "id": 3,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "STRING",
                                   "value": "root"
                               },
                               {
                                   "size": 40,
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.password",
                                   "id": 4,
                                   "sensitive": true,
                                   "overrides": "",
                                   "type": "STRING"
                               },
                               {
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.fetchSize",
                                   "id": 5,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "INTEGER"
                               },
                               {
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.jdbcProperties",
                                   "id": 6,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "MAP",
                                   "sensitive-pattern": ""
                               }
                           ],
                           "name": "linkConfig",
                           "id": 1,
                           "type": "LINK"
                       },
                       {
                           "validators": [],
                           "inputs": [
                               {
                                   "size": 5,
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "dialect.identifierEnclose",
                                   "id": 7,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "STRING",
                                   "value": "+"
                               }
                           ],
                           "name": "dialect",
                           "id": 2,
                           "type": "LINK"
                       }
                   ],
                   "validators": []
               },
               "creation-user": "hadoop",
               "name": "mysql",
               "creation-date": 1558459776286,
               "connector-name": "generic-jdbc-connector",
               "id": 1,
               "update-date": 1558946713339,
               "enabled": true,
               "update-user": "hadoop"
           }
       ]
   }
   ```

   

8. **/v1/link - [POST] - Create Link**

   URL

   ```
   http://192.168.56.100:12000/sqoop/v1/link
   ```

   

   Request json:

   ```json
   {
       "links": [
           {
               "link-config-values": {
                   "configs": [
                       {
                           "validators": [],
                           "inputs": [
                               {
                                   "size": 255,
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.uri",
                                   "id": 65,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "STRING",
                                   "value": "hdfs%3A%2F%2Fmaster"
                               },
                               {
                                   "size": 255,
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.confDir",
                                   "id": 66,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "STRING",
                                   "value": "%2Fusr%2Flocal%2Fhadoop%2Fetc%2Fhadoop"
                               },
                               {
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.configOverrides",
                                   "id": 67,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "MAP",
                                   "sensitive-pattern": ""
                               }
                           ],
                           "name": "linkConfig",
                           "id": 14,
                           "type": "LINK"
                       }
                   ],
                   "validators": []
               },
               "creation-user": "hadoop",           
               "name": "testLink123",               
               "creation-date": 1559102718234,
               "connector-name": "hdfs-connector",
               "id": -1,
               "update-date": 1559102718974,
               "enabled": true,
               "update-user": "hadoop"
           }
       ]
   }
   ```

   參數介紹:

   **creation-user** , **update-user** 填使用者的帳號

   **name** 填你要為此Link的名字

   **connector-name** 填入connector的類型

   **id** 填-1會自動產生id

   **creation-date** , **update-date** 請填入時間

   **enabled** 請填true

   **link-config-values** 請填入相對應的connector所需的資料

   Response

   ```json
   {
       "name": "testLink123",
       "validation-result": [
           {}
       ]
   }
   ```

9. **/v1/link/[lname] - [PUT] - Update Link**

   URL

   ```
   http://192.168.56.100:12000/sqoop/v1/link/testLink123
   ```

   

   Request

   ```json
   {
       "links": [
           {
               "link-config-values": {
                   "configs": [
                       {
                           "validators": [],
                           "inputs": [
                               {
                                   "size": 255,
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.uri",
                                   "id": 65,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "STRING",
                                   "value": "hdfs%3A%2F%2Fmaster"
                               },
                               {
                                   "size": 255,
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.confDir",
                                   "id": 66,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "STRING",
                                   "value": "%2Fusr%2Flocal%2Fhadoop%2Fetc"
                               },
                               {
                                   "editable": "ANY",
                                   "validators": [],
                                   "name": "linkConfig.configOverrides",
                                   "id": 67,
                                   "sensitive": false,
                                   "overrides": "",
                                   "type": "MAP",
                                   "sensitive-pattern": ""
                               }
                           ],
                           "name": "linkConfig",
                           "id": 14,
                           "type": "LINK"
                       }
                   ],
                   "validators": []
               },
               "creation-user": "hadoop",
               "name": "testLink123",
               "creation-date": 1559102718234,
               "connector-name": "hdfs-connector",
               "id": -1,
               "update-date": 1559102718974,
               "enabled": true,
               "update-user": "hadoop"
           }
       ]
   }
   ```

   備註:這邊我更改了Conf directory的值 , 原本是"%2Fusr%2Flocal%2Fhadoop%2Fetc%2Fhadoop"然後改成"%2Fusr%2Flocal%2Fhadoop%2Fetc"

   Response json:

   ```json
   {
       "validation-result": [
           {}
       ]
   }
   ```

10. **/v1/link/[lname] - [DELETE] - Delete Link**

    URL

    ```
    http://192.168.56.100:12000/sqoop/v1/link/testLink123
    ```

    Response

    ```json
    {}
    ```

11. **/v1/link/[lname]/enable - [PUT] - Enable Link**

    URL

    ```
    http://192.168.56.100:12000/sqoop/v1/link/testLink123/enable
    ```

    Response

    ```json
    {}
    ```

12. **/v1/link/[lname]/disable - [PUT] - Disable Link**

    URL

    ```
    http://192.168.56.100:12000/sqoop/v1/link/testLink123/disable
    ```

    Response

    ```json
    {}
    ```

13. /v1/jobs

14. /v1/jobs?cname=[cname]

15. **/v1/job/[jname] - [GET] - Get Job**

    URL

    ```
    http://192.168.56.100:12000/sqoop/v1/job/test31
    ```

    Response

    ```json
    {
        "jobs": [
            {
                "to-config-values": {
                    "configs": [
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.overrideNullValue",
                                    "id": 73,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "BOOLEAN"
                                },
                                {
                                    "size": 255,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.nullValue",
                                    "id": 74,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "values": "TEXT_FILE,SEQUENCE_FILE,PARQUET_FILE",
                                    "name": "toJobConfig.outputFormat",
                                    "id": 75,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "ENUM",
                                    "value": "TEXT_FILE"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "values": "NONE,DEFAULT,DEFLATE,GZIP,BZIP2,LZO,LZ4,SNAPPY,CUSTOM",
                                    "name": "toJobConfig.compression",
                                    "id": 76,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "ENUM",
                                    "value": "NONE"
                                },
                                {
                                    "size": 255,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.customCompression",
                                    "id": 77,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                },
                                {
                                    "size": 255,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.outputDirectory",
                                    "id": 78,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING",
                                    "value": "%2Fuser%2Ftest31"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.appendMode",
                                    "id": 79,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "BOOLEAN"
                                }
                            ],
                            "name": "toJobConfig",
                            "id": 17,
                            "type": "JOB"
                        }
                    ],
                    "validators": []
                },
                "from-config-values": {
                    "configs": [
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.schemaName",
                                    "id": 8,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING",
                                    "value": "hidb"
                                },
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.tableName",
                                    "id": 9,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING",
                                    "value": "mytable5"
                                },
                                {
                                    "size": 2000,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.sql",
                                    "id": 10,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.columnList",
                                    "id": 11,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "LIST"
                                },
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.partitionColumn",
                                    "id": 12,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING",
                                    "value": "id"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.allowNullValueInPartitionColumn",
                                    "id": 13,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "BOOLEAN"
                                },
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.boundaryQuery",
                                    "id": 14,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                }
                            ],
                            "name": "fromJobConfig",
                            "id": 3,
                            "type": "JOB"
                        },
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "incrementalRead.checkColumn",
                                    "id": 15,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                },
                                {
                                    "size": -1,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "incrementalRead.lastValue",
                                    "id": 16,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                }
                            ],
                            "name": "incrementalRead",
                            "id": 4,
                            "type": "JOB"
                        }
                    ],
                    "validators": []
                },
                "to-connector-name": "hdfs-connector",
                "from-link-name": "mysql",
                "enabled": true,
                "from-connector-name": "generic-jdbc-connector",
                "to-link-name": "hdfs",
                "driver-config-values": {
                    "configs": [
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "throttlingConfig.numExtractors",
                                    "id": 88,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "INTEGER"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "throttlingConfig.numLoaders",
                                    "id": 89,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "INTEGER"
                                }
                            ],
                            "name": "throttlingConfig",
                            "id": 22,
                            "type": "JOB"
                        },
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "jarConfig.extraJars",
                                    "id": 90,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "LIST"
                                }
                            ],
                            "name": "jarConfig",
                            "id": 23,
                            "type": "JOB"
                        }
                    ],
                    "validators": []
                },
                "creation-user": "hadoop",
                "name": "test31",
                "creation-date": 1559033509101,
                "id": 20,
                "update-date": 1559033509101,
                "update-user": "hadoop"
            }
        ]
    }
    ```

16. **/v1/job -[POST] - Create Job**

    URL

    ```
    http://192.168.56.100:12000/sqoop/v1/job
    ```

    Request

    ```json
    {
        "jobs": [
            {
                "to-config-values": {
                    "configs": [
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.overrideNullValue",
                                    "id": 73,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "BOOLEAN"
                                },
                                {
                                    "size": 255,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.nullValue",
                                    "id": 74,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "values": "TEXT_FILE,SEQUENCE_FILE,PARQUET_FILE",
                                    "name": "toJobConfig.outputFormat",
                                    "id": 75,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "ENUM",
                                    "value": "TEXT_FILE"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "values": "NONE,DEFAULT,DEFLATE,GZIP,BZIP2,LZO,LZ4,SNAPPY,CUSTOM",
                                    "name": "toJobConfig.compression",
                                    "id": 76,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "ENUM",
                                    "value": "NONE"
                                },
                                {
                                    "size": 255,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.customCompression",
                                    "id": 77,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                },
                                {
                                    "size": 255,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.outputDirectory",
                                    "id": 78,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING",
                                    "value": "%2Fuser%2Ftest31"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.appendMode",
                                    "id": 79,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "BOOLEAN"
                                }
                            ],
                            "name": "toJobConfig",
                            "id": 17,
                            "type": "JOB"
                        }
                    ],
                    "validators": []
                },
                "from-config-values": {
                    "configs": [
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.schemaName",
                                    "id": 8,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING",
                                    "value": "hidb"
                                },
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.tableName",
                                    "id": 9,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING",
                                    "value": "mytable5"
                                },
                                {
                                    "size": 2000,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.sql",
                                    "id": 10,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.columnList",
                                    "id": 11,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "LIST"
                                },
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.partitionColumn",
                                    "id": 12,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING",
                                    "value": "id"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.allowNullValueInPartitionColumn",
                                    "id": 13,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "BOOLEAN"
                                },
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.boundaryQuery",
                                    "id": 14,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                }
                            ],
                            "name": "fromJobConfig",
                            "id": 3,
                            "type": "JOB"
                        },
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "incrementalRead.checkColumn",
                                    "id": 15,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                },
                                {
                                    "size": -1,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "incrementalRead.lastValue",
                                    "id": 16,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                }
                            ],
                            "name": "incrementalRead",
                            "id": 4,
                            "type": "JOB"
                        }
                    ],
                    "validators": []
                },
                "to-connector-name": "hdfs-connector",
                "from-link-name": "mysql",
                "enabled": true,
                "from-connector-name": "generic-jdbc-connector",
                "to-link-name": "hdfs",
                "driver-config-values": {
                    "configs": [
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "throttlingConfig.numExtractors",
                                    "id": 88,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "INTEGER"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "throttlingConfig.numLoaders",
                                    "id": 89,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "INTEGER"
                                }
                            ],
                            "name": "throttlingConfig",
                            "id": 22,
                            "type": "JOB"
                        },
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "jarConfig.extraJars",
                                    "id": 90,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "LIST"
                                }
                            ],
                            "name": "jarConfig",
                            "id": 23,
                            "type": "JOB"
                        }
                    ],
                    "validators": []
                },
                "creation-user": "hadoop",
                "name": "testAPI",
                "creation-date": 1559102718234,
                "id": -1,
                "update-date": 1559102718974,
                "update-user": "hadoop"
            }
        ]
    }
    ```

    Response

    ```json
    {
        "name": "testAPI",
        "validation-result": [
            {},
            {},
            {}
        ]
    }
    ```

17. **/v1/job/[jname] - [PUT] - Update Job**

    URL

    ```
    http://192.168.56.100:12000/sqoop/v1/job/testAPI
    ```

    Request

    ```json
    {
        "jobs": [
            {
                "to-config-values": {
                    "configs": [
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.overrideNullValue",
                                    "id": 73,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "BOOLEAN"
                                },
                                {
                                    "size": 255,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.nullValue",
                                    "id": 74,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "values": "TEXT_FILE,SEQUENCE_FILE,PARQUET_FILE",
                                    "name": "toJobConfig.outputFormat",
                                    "id": 75,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "ENUM",
                                    "value": "TEXT_FILE"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "values": "NONE,DEFAULT,DEFLATE,GZIP,BZIP2,LZO,LZ4,SNAPPY,CUSTOM",
                                    "name": "toJobConfig.compression",
                                    "id": 76,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "ENUM",
                                    "value": "NONE"
                                },
                                {
                                    "size": 255,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.customCompression",
                                    "id": 77,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                },
                                {
                                    "size": 255,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.outputDirectory",
                                    "id": 78,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING",
                                    "value": "%2Fuser%2Ftest31"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "toJobConfig.appendMode",
                                    "id": 79,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "BOOLEAN",
                                    "value": "true"
                                }
                            ],
                            "name": "toJobConfig",
                            "id": 17,
                            "type": "JOB"
                        }
                    ],
                    "validators": []
                },
                "from-config-values": {
                    "configs": [
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.schemaName",
                                    "id": 8,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING",
                                    "value": "hidb"
                                },
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.tableName",
                                    "id": 9,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING",
                                    "value": "mytable5"
                                },
                                {
                                    "size": 2000,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.sql",
                                    "id": 10,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.columnList",
                                    "id": 11,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "LIST"
                                },
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.partitionColumn",
                                    "id": 12,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING",
                                    "value": "id"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.allowNullValueInPartitionColumn",
                                    "id": 13,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "BOOLEAN"
                                },
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "fromJobConfig.boundaryQuery",
                                    "id": 14,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING"
                                }
                            ],
                            "name": "fromJobConfig",
                            "id": 3,
                            "type": "JOB"
                        },
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "size": 50,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "incrementalRead.checkColumn",
                                    "id": 15,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING",
                                    "value": "id"
                                },
                                {
                                    "size": -1,
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "incrementalRead.lastValue",
                                    "id": 16,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "STRING",
                                    "value": "1003"
                                }
                            ],
                            "name": "incrementalRead",
                            "id": 4,
                            "type": "JOB"
                        }
                    ],
                    "validators": []
                },
                "to-connector-name": "hdfs-connector",
                "from-link-name": "mysql",
                "enabled": true,
                "from-connector-name": "generic-jdbc-connector",
                "to-link-name": "hdfs",
                "driver-config-values": {
                    "configs": [
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "throttlingConfig.numExtractors",
                                    "id": 88,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "INTEGER"
                                },
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "throttlingConfig.numLoaders",
                                    "id": 89,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "INTEGER"
                                }
                            ],
                            "name": "throttlingConfig",
                            "id": 22,
                            "type": "JOB"
                        },
                        {
                            "validators": [],
                            "inputs": [
                                {
                                    "editable": "ANY",
                                    "validators": [],
                                    "name": "jarConfig.extraJars",
                                    "id": 90,
                                    "sensitive": false,
                                    "overrides": "",
                                    "type": "LIST"
                                }
                            ],
                            "name": "jarConfig",
                            "id": 23,
                            "type": "JOB"
                        }
                    ],
                    "validators": []
                },
                "creation-user": "hadoop",
                "name": "testAPI",
                "creation-date": 1559102718234,
                "id": -1,
                "update-date": 1559120997185,
                "update-user": "hadoop"
            }
        ]
    }
    ```

    Response

    ```json
    {
        "validation-result": [
            {},
            {},
            {}
        ]
    }
    ```

18. **/v1/job/[jname] - [DELETE] - Delete Job**

    URL

    ```
    http://192.168.56.100:12000/sqoop/v1/job/testAPI
    ```

    Response

    ```json
    {}
    ```

19. **/v1/job/[jname]/enable - [PUT] - Enable Job**

    URL

    ```
    http://192.168.56.100:12000/sqoop/v1/job/testAPI/enable
    ```

    Response

    ```json
    {}
    ```

    

20. **/v1/job/[jname]/disable - [PUT] - Disable Job**

    URL

    ```
    http://192.168.56.100:12000/sqoop/v1/job/testAPI/disable
    ```

    Response

    ```json
    {}
    ```

21. **/v1/job/[jname]/start - [PUT] - Start Job**

    URL

    ```
    http://192.168.56.100:12000/sqoop/v1/job/testAPI1/start
    ```

    Response

    ```json
    {
        "submissions": [
            {
                "job-name": "testAPI",
                "creation-user": "sqoop.anonymous.user",
                "progress": -1,
                "creation-date": 1559122356895,
                "external-id": "job_1559094861049_0002",
                "last-update-date": 1559122356895,
                "last-udpate-user": "sqoop.anonymous.user",
                "external-link": "http://master:8088/proxy/application_1559094861049_0002/",
                "from-schema": {
                    "created": 1559122357336,
                    "columns": [
                        {
                            "nullable": true,
                            "charSize": null,
                            "name": "name",
                            "type": "TEXT"
                        },
                        {
                            "nullable": true,
                            "byteSize": 4,
                            "name": "id",
                            "signed": true,
                            "type": "FIXED_POINT"
                        }
                    ],
                    "name": " hidb . mytable5 "
                },
                "to-schema": {
                    "created": 1559120468229,
                    "columns": [],
                    "name": "NullSchema"
                },
                "status": "BOOTING"
            }
        ]
    }
    ```

22. /v1/job/[jbname]/stop - [PUT] - Stop Job

    URL

    ```
    
    ```

    

23. **/v1/job/[jbname]/status - [GET] - Get Job Status**

    URL

    ```
    http://192.168.56.100:12000/sqoop/v1/job/test34/status
    ```

    Response

    ```json
    {
        "submissions": [
            {
                "job-name": "test34",
                "creation-user": "sqoop.anonymous.user",
                "progress": 0,
                "creation-date": 1559123136975,
                "external-id": "job_1559094861049_0004",
                "last-update-date": 1559123174747,
                "last-udpate-user": "sqoop.anonymous.user",
                "external-link": "http://master:8088/proxy/application_1559094861049_0004/",
                "status": "RUNNING"
            }
        ]
    }
    ```

24. **/v1/submissions? - [GET] - Get All Job Submissions**

    URL

    ```
    http://192.168.56.100:12000/sqoop/v1/submissions?
    ```

    Response

    ```
     {
                "job-name": "testAPI",
                "creation-user": "hadoop",
                "progress": -1,
                "creation-date": 1559120466632,
                "external-id": "job_1559094861049_0001",
                "last-update-date": 1559120466635,
                "last-udpate-user": "hadoop",
                "external-link": "http://master:8088/proxy/application_1559094861049_0001/",
                "status": "BOOTING"
            },
    ```

25. **/v1/submissions? jname=[jname] - [GET] - Get Submissions By Job**

    URL

    ```
    http://192.168.56.100:12000/sqoop/v1/submissions?jname=testAPI
    ```

    Response

    ```
    {
        "submissions": [
            {
                "job-name": "testAPI",
                "creation-user": "hadoop",
                "progress": -1,
                "creation-date": 1559033548402,
                "external-id": "job_1559008614044_0009",
                "last-update-date": 1559033601953,
                "last-udpate-user": "hadoop",
                "external-link": "http://master:8088/proxy/application_1559008614044_0009/",
                "status": "RUNNING"
            }
        ]
    }
    ```

    

