# vuepress + github pages

TIL 작성할 때 vuepress로 많이 만들길래 따라해봄!

## 설치

1. github repository 생성 후 본인은 blog로 생성함.
2. 로컬 환경에서 폴더 생성 후 해당 경로로 이동.

```sh
$ mkdir blog
$ cd blog
$ git clone git@github.com:<USER_ID>/<REPO>.git .
```

3. clone 받은 후 vuepress, npm 초기 설정 진행

```sh
$ npm init
$ npm install -D vuepress
```

4. 루트 경로에 `package.json` 의 `script` 구문을 다음과 같이 수정

```json
  ...
  "scripts": {
    "docs:dev": "vuepress dev docs",
    "docs:build": "vuepress build docs"
  },
  ...
```

5. 루트 경로에 `docs` 폴더 생성 후 `README.md` 생성 후 작성

```md
# Hello World
```

6. 아래 명령어 실행 후 작업된 내용을 확인해본다. `localhost:8080/blog/`

```sh
$ npm run docs:dev
```

7. 루트경로에 `deploy.sh` 생성 후 아래 내용 작성

```sh
#!/usr/bin/env sh

# abort on errors
set -e

git pull
git add -A
git commit -m "$1 $2 $3 --all.sh master"
# origin은 프로젝트 설정에 맞게 수정
git push origin main

# build
npm run docs:build
# navigate into the build output directory
cd docs/.vuepress/dist

# if you are deploying to a custom domain
# echo 'www.example.com' > CNAME

git init
git add -A
git commit -m 'deploy with vuepress'

# if you are deploying to https://<USERNAME>.github.io
# git push -f git@github.com:<USERNAME>/<USERNAME>.github.io.git master

# if you are deploying to https://<USERNAME>.github.io/<REPO>
git push -f git@github.com:<USER_ID>/<REPO>.git master:gh-pages

cd -
```

8. 빌드도 한번 돌려보자. 빌드가 성공하면 docs/ 경로에 `.vuepress` 폴더가 생성된다.

```sh
$ npm run docs:build
```

9. `.vuepress`경로에 `config.js` 파일 생성 후 다음과 같이 작성

```js
module.exports = {
  title: "title",
  description: "desc",
  themeConfig: {
    nav: [{ text: "Github", link: "https://github.com/hhk2745" }],
    sidebar: getSidebarArr(),
  },
  //가장 중요한 부분!
  //<username>.github.io 뒤에 주소가 붙으시면
  //아래와 같이 뒤 붙는 주소를 넣어주셔야합니다.
  //안그러면 css 가 반영이 안되요!! 꼭꼭 넣어주세요
  base: "/blog/",
};

function getSidebarArr() {
  var fs = require("fs");
  var docsPath = __dirname + "/../";
  var sidebarArr = [];
  var HomeFilelist = [];
  var filelist = fs.readdirSync(docsPath);
  filelist.forEach(function (file) {
    if (file === ".vuepress") return;
    var stat = fs.lstatSync(docsPath + "/" + file);
    if (stat.isDirectory()) {
      // directory
      // title is file, children is readdirSync
      var docsFolderPath = docsPath + "/" + file;
      var list = fs.readdirSync(docsFolderPath);
      sidebarArr.push(makeSidebarObject(file, list));
    } else {
      // NOT directory
      // title is '/' children is file
      HomeFilelist.push(file);
    }
  });
  sidebarArr.unshift(makeSidebarObject("", HomeFilelist));
  return sidebarArr;
}
function makeSidebarObject(folder, mdfileList) {
  var path = folder ? "/" + folder + "/" : "/";
  mdfileList = aheadOfReadme(mdfileList);
  var tmpMdfileList = [];
  // remove .md, add Path
  mdfileList.forEach(function (mdfile) {
    if (mdfile.substr(-3) === ".md") {
      mdfile = mdfile.slice(0, -3) === "README" ? "" : mdfile.slice(0, -3);
      tmpMdfileList.push(path + mdfile);
    }
  });
  mdfileList = tmpMdfileList;
  // remove folder prefix number
  if (folder) {
    var dotIdx = folder.indexOf(".");
    var title = Number(folder.substr(0, dotIdx))
      ? folder.substr(dotIdx + 1)
      : folder;
  } else {
    title = "HOME";
  }
  return {
    title: title,
    children: mdfileList,
  };
}
function aheadOfReadme(arr) {
  // ['1.test.md','README.md'] => ['README.md','1.test.md']
  var readmeIdx = arr.indexOf("README.md");
  if (readmeIdx > 0) {
    arr.unshift(arr.splice(readmeIdx, 1)[0]);
  }
  return arr;
}
```

10. 프로젝트 루트경로에서 만들어진 스크립트를 실행해보자

```sh
$ sh ./deploy.sh
```

11. `https://github.com/<USER_ID>/<REPO>` Actions 탭에서 deploy 결과를 확인할 수 있다. (실시간 반영은 아니므로 5분 내로 확인 가능)

[참고사이트](https://kyounghwan01.github.io/blog/Vue/vuepress/vuepress-start/)
