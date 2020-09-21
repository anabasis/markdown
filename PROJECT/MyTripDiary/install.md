# MyTripDiary

## Node 삭제

npm config get prefix
cd [ 위 명령으로 나온 경로. ex) /Users/gomugom/.npm-packages ]
cd lib && rm -rf node_modules

lsbom -f -l -s -pf /var/db/receipts/org.nodejs.node.pkg.bom | while read f; do  sudo rm /usr/local/${f}; done
sudo rm -rf /usr/local/lib/node /usr/local/lib/node_modules /var/db/receipts/org.nodejs.*

cd /usr/local/lib && sudo rm -rf node*
cd /usr/local/include && sudo rm -rf node*

cd /usr/local/bin
sudo rm -rf npm
sudo rm -rf node

sudo rm -rf /usr/local/share/man/man1/node.1
sudo rm -rf /usr/local/lib/dtrace/node.d
sudo rm -rf ~/.npm

rm -fr /usr/local/bin/{node,npm} /usr/local/lib/node_modules/

sudo rm -rf /usr/local/lib/node /usr/local/lib/node_modules /var/db/receipts/org.nodejs.*
sudo rm -rf /usr/local/include/node /Users/$USER/.npm
sudo rm /usr/local/bin/node
sudo rm /usr/local/share/man/man1/node.1
sudo rm /usr/local/lib/dtrace/node.d

cd /usr/local/lib
sudo rm -rf node*
cd /usr/local/include
sudo rm -rf node*

sudo rm /usr/local/bin/npm
sudo rm /usr/local/share/man/man1/node.1
sudo rm /usr/local/lib/dtrace/node.d
sudo rm -rf ~/.npm
sudo rm -rf ~/.node-gyp
sudo rm /opt/local/bin/node
sudo rm /opt/local/include/node
sudo rm -rf /opt/local/lib/node_modules

brew uninstall node --force
sudo npm uninstall npm -g

## 설치

curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
brew install nvm

nvm install 10.16.0

<https://trustyoo86.github.io/nodejs/2019/02/18/ubuntu-nvm.html>

brew install node
brew install watchman
brew install flow

<https://medium.com/@jang.wangsu/rn-react-native-%EC%8B%9C%EC%9E%91-3aab881f574f>
<https://jsdev.kr/t/react-native-style-atom-package/2428>
<http://suhanlee.github.io/2017/react-native-guide-1.html>

cd /Users/chojunseung/Workings/Repos/git/nodejs/REACT-NATIVE
react-native init MyTripDiary

react-native run-ios
react-native run-android

