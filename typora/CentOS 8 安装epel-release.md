# CentOS 8 安装epel-release

1.  使用dnf安装epel-release
2.  安装subscription-manager
3.  启用codeready-builder-for-rhel-8-*-rpms repository
4.  启用PowerTools repository

## 安装epel-release

```shell
# dnf install -y epel-release
```

## 安装subscription-manager

```shell
# dnf install -y subscription-manager
```

## 启用codeready-builder-for-rhel-8-*-rpms repository

需要使用一个变量来获取系统的架构。

```shell
# ARCH=$( /bin/arch )
# subscription-manager repos --enable "codeready-builder-for-rhel-8-${ARCH}-rpms"
```

然而出现了“This system has no repositories available through subscriptions.”，目前不知道有什么问题。

## 启用PowerTools repository

```shell
# dnf config-manager --set-enabled PowerTools
```

然而提示“This system is not registered to Red Hat Subscription Management. You can use subscription-manager to register.”，没有注册到red hat，那我没办法了。

先不管那么多了，epel先用着吧，虽然可能会有依赖问题 ，等遇到了再解决。