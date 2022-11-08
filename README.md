#基于docker的矩阵图像


详情见:[matrix2](https://github.com/xvmvx/docker-matrix-2)
Dockerfile用于安装[matrix]开放式联合即时消息和VoIP通信服务器。

- [compose](https://github.com/corpusops/setups.matrix)
- [coturn](https://github.com/coturn/coturn)
- [matrix img](https://github.com/corpusops/docker-matrix)
- [matrix](https://matrix.org)
- [riot](https://github.com/corpusops/docker-riot)

#配置
要进行配置，请使用“generate”作为参数运行图像。您必须设置服务器域和“/data”目录。之后，您必须编辑生成了homeserver.yaml文件。
为了完成这些工作，“generate”将创建一个自己的自签名证书。>这需要根据生产用途进行更改。
例子：

    $ docker run -v /tmp/data:/data --rm -e SERVER_NAME=localhost -e REPORT_STATS=no corpusops/docker-matrix generate
    
    

#开始
为了启动，您需要端口绑定和`/data`-目录。

    $ docker run -d -p 8448:8448 -p 3478:3478 -v /tmp/data:/data corpusops/docker-matrix start

#端口配置
##矩阵家庭服务器
以下端口用于Matrix服务器的容器中。您可以在上使用“-p”选项
`docker run` to configure this part (eg.: `-p 443:8448`):`8008,8448 tcp`


#版本信息
要获得已安装的synapse版本，可以使用“version”运行映像或者通过cat查看容器。

    $ docker run -ti --rm corpusops/docker-matrix version
    -=> Matrix Version
    synapse: master (7e0a1683e639c18bd973f825b91c908966179c15)

    # docker exec -it CONTAINERID cat /synapse.version
    synapse: master (7e0a1683e639c18bd973f825b91c908966179c15)


#环境变量

*`SERVER_NAME`：      服务器和域名，必填，仅用于`generate`
*`REPORT_STATS`：    统计报告，必填，值：“是”或“否”，需要
仅用于`generate`
*`MATRIX_UID`/`MATRIX_GID`：    运行synapse服务器的容器中用户的UserID和GroupID。在/data下装载的文件被“吃掉”到该所有权。默认值为`MATRIX_UID=991`和`MATRIX_GID=991'。它可以在开始时通过`-e MATRIX_UID=…`和`-e MATRIX_GID=…'重写。
#生成特定参数
*`BV_SYN`：    突触版本，可选，默认为`master`对于使用commit a9fc47e构建synapse版本v0.11.0-rc2，请添加`--build-arg BV_SYN=v0.11.0-rc2 to the `docker
build` command.
#系统和新生成的配置文件之间的差异
要获得有关新选项等的提示，您可以在配置的主服务器。yaml和一个新创建的配置文件。将带有“diff”的图像称为论点


```
$ docker run --rm -ti -v /tmp/data:/data corpusops/docker-matrix diff
[...]
+# ldap_config:
+#   enabled: true
+#   server: "ldap://localhost"
+#   port: 389
+#   tls: false
+#   search_base: "ou=Users,dc=example,dc=com"
+#   search_property: "cn"
+#   email_property: "email"
+#   full_name_property: "givenName"
[...]
```

为了生成此输出，使用了来自“busybox”的“diff”。使用的差异可以通过“DIFFPARAMS”环境变量更改参数。这个默认值为“Naur”。


#导出的卷

* `/data`: data-container
