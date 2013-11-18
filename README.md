packer_redhat_chef
==================

packer
-------------
http://dev.classmethod.jp/cloud/aws/create_amazon-ami_by_packer/

> 一つの設定ファイルから複数の環境に同時にマシンイメージを作成する
複数環境ではなくAWS EC2/AMIを管理するケースでも、AMIを自動作成し、その構成内容をコード(設定ファイル)で管理できるというEC2/AMIの新しい管理手法として非常に有用
AMIは他人が作ったものはもとより、開発者が自分で作ったものであっても、作成したあとからAMIの内容を正確に把握することが難しいブラックボックスになりがち
Packerを使用すれば、設定ファイル(後述のTemplate)をGitなどのVCSで管理することで、AMIの構成をコードで管理することが可能

準備するもの
-------------
- AWSのアクセスキーとシークレットキーを、[AWSのMy Account => セキュリティ証明書 画面](https://portal.aws.amazon.com/gp/aws/securityCredentials)で、確認する

- AMI IDを[AWSのLaunchInstanceWizard](https://console.aws.amazon.com/ec2/v2/home?region=ap-northeast-1#LaunchInstanceWizard)で、確認する

`Red Hat Enterprise Linux 6.4 - ami-5769f956 (64-bit) / ami-bb68f8ba (32-bit)`


template 作り
==============
アクセスキーを外だしする
--------------
[アクセスキーとシークレットキーは環境変数として外だしすべき](http://dev.classmethod.jp/cloud/packer-tips-for-ci-tools/)



chefが使える環境をプロビジョニング
-------------

- ruby をインストール
- [sudoできない](http://open-groove.net/linux/sudo-requiretty/)事象への対処のため、/etc/sudoersを書き換える 
- - [ruby ワンライナーの使い方まとめ](http://takuya-1st.hatenablog.jp/entry/2013/08/19/194819)
- chef をインストール 
- - [[速報]Packerでさまざまな仮想マシンのテンプレートを作成する](http://www.ryuzee.com/contents/blog/6697)

- packer build
 
    
    time packer build -var 'aws_access_key=...' -var 'aws_secret_key=...' redhat-chef-solo.json
    
    
3分から5分くらいでできあがる

ここでできあがったAMIをもとに、vagrant up すれば、楽しい世界が待っている
