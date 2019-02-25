## 安装 sourcetree
https://link.juejin.im/?target=https%3A%2F%2Fdownloads.atlassian.com%2Fsoftware%2Fsourcetree%2FSourcetree_2.6.3a.zip

## 安装git
` brew install git `
```
// 配置git
git config --global user.name "lirunkai"
git config --global user.email "i_lirunkai@cvte.com"
// 通过终端命令创建ssh key
ssh-keygen -t rsa -C "i_lirunkai@cvte.com"
// 一路回车
// ~/ 下面
cat .ssh/id_rsa.pub 
// 复制，粘贴到git到ssh上面
// 回去 sourcetree 新建 从URL克隆（复制git上面到ssh链接）
```


