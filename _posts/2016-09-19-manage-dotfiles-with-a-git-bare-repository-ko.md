---
layout: post
ref: manage-dotfiles
date:   2016-09-19 00:00:00 +0900
title: Bare Git Repository 사용해서 설정파일 관리하기
lang: ko
---

서브라임 텍스트를 사용하다 빔으로 완전 전환한 이후, 설정용 닷파일을 관리할 필요가 생겼습니다. 만들고 이해하는데 시간도 많이 걸려서 혹시나 모를 사태에 대비해 저장도 하고 싶어졌고, 다른 시스템을 사용해도 언제든 받아서 사용할 수 있도록 해두고 싶었습니다.

처음엔 vagrant를 사용할까 했지만 겨우 파일 몇 개 관리하는데 사용하기엔 오버인 것 같고, 깃 버전 관리용 디렉토리를 하나 따로 만들어서 닷파일을 심링크해서 관리할까 했는데 이것도 쓸데없이 복잡하게 느껴졌습니다. 그래서 분명 더 나을 방법이 있을 거라고 생각하고 찾아봤는데...

제 마음에 꼭 드는 것을 찾았습니다. <a href="http://www.saintsjd.com/2011/01/what-is-a-bare-git-repository/">Git bare repository</a>를 사용하고, 간단한 앨리어싱을 해주는 방법입니다. 설정하기도 간단하고, 깃으로 버전 관리도 되고, 다른 시스템에 설치하기도 쉽고, 제가 필요로 하는 것을 정확히 충족시켜주네요.

### 설정

깃만 있으면 되고, 명령 네 줄만 입력하면 끝납니다.

{% highlight bash linenos %}
git init --bare $HOME/.config
echo "alias config='/usr/bin/git --git-dir=$HOME/.config/ --work-tree=$HOME'" >> $HOME/.zshrc
reload ~/.zshrc
config config --local status.showUntrackedFiles no
{% endhighlight %}

1. ~/.config 에 bare git repository 를 생성하고
2. 쉘 설정 파일에 앨리어싱 설정을 넣어줍니다. 저는 zsh 를 사용해서 .zshrc 지만 bash 사용자는 .bashrc 겠죠? --git-dir 와 --work-tree 패스를 눈여겨보세요. 
3. 설정 파일 적용을 위해 리로딩 해줍니다.
4. status 커맨드를 입력했을 때 트랙하도록 설정하지 않은 파일이 보이지 않도록 해줍니다.

이제 홈 디렉토리에서 앨리어싱 해둔 커맨드를 사용해서 파일을 관리하고, 필요하면 리모트 리포를 설정해서 사용하면 됩니다.

{% highlight bash linenos %}
config status
config add .vimrc
config commit -m "Add vimrc"
config remote add origin https://www.github.com/username/repo.git
config push origin master
{% endhighlight %}

### 다른 시스템에 닷파일 가져오기

리모트 리포에서 파일을 가져오기 전에 쉘 커맨드 두 줄만 입력하면 됩니다.

{% highlight bash linenos %}
echo "alias config='/usr/bin/git --git-dir=$HOME/.config/ --work-tree=$HOME'" >> $HOME/.zshrc
reload ~/.zshrc
echo ".config" >> .gitignore
git clone --bare https://www.github.com/username/repo.git $HOME/.config
config checkout
config config --local status.showUntrackedFiles no
{% endhighlight %}

1. Git bare repository 가 문제없이 동작하도록 앨리어스를 설정해줍니다.
2. 설정 파일 적용을 위해 리로드.
3. .gitignore 에 .config 디렉토리를 추가해줍니다. 리커젼 문제를 방지하기 위해서입니다.
4. 리모트 리포 클론.
5. 제대로 가져왔는지 확인하고,
6. 마찬가지로 트랙하도록 설정해두지 않은 파일이 status 커맨드 결과에 보이지 않도록 해줍니다.

간단하죠? 이런 훌륭한 생각을 한 선배들에게 감사드리고, 깔끔하게 내용 및 단계 정리한 Nicola Paolucci 에게도 감사드립니다. 원본 글 링크는 <a href="https://developer.atlassian.com/blog/2016/02/best-way-to-store-dotfiles-git-bare-repo/">여기로</a>!
