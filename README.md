# Submodule-test

git submodule で、operation-check/GitHub-Wiki-test-public のsubmodule化を実施。
localでの作業。

・sub moduleの登録
1.Submodule-testをclone
2.gitbushなど、ターミナルを使用して  git submodule add <URL> 
```
$ git submodule add https://github.com/operation-check/GitHub-Wiki-test-public.git
Cloning into 'C:/Users/k-aoki/Source/Git/Submodule-test/GitHub-Wiki-test-public'...
remote: Enumerating objects: 93, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 93 (delta 0), reused 0 (delta 0), pack-reused 90
Receiving objects: 100% (93/93), 16.27 KiB | 1.48 MiB/s, done.
Resolving deltas: 100% (17/17), done.
```

localのフォルダ表示
![image](https://user-images.githubusercontent.com/85093305/148142519-0cf7aced-c723-499d-8390-c0b2594066e9.png)

この状態でsubmodule化はできている。
（組み込んでいるのは、最新ハッシュ（13dc99e））

ここからcommit→pushすればGitHub上でも「特定ハッシュに対してのみ」module化が可能。
また、組み込めるrepositoryについてはアクセス権の問題はあるかも知れないが、private repositoryでも可能だった。
 

・sub moduleの更新
  submoduleは特定ハッシュに対して行うため、基本的に自動更新はされない。
  このため定期的に最新化を行う必要がある。

更新はlocalで   git submodule update --remote
```
$ git submodule update --remote
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 942 bytes | 78.00 KiB/s, done.
From https://github.com/operation-check/GitHub-Wiki-test-public
   13dc99e..bb20d0d  main       -> origin/main
Submodule path 'GitHub-Wiki-test-public': checked out 'bb20d0d36411cd0f276af884310140831401d558'
```

GitHub上の表示
![image](https://user-images.githubusercontent.com/85093305/148143221-091aeb52-9c30-4767-bfab-6a6f2ac64c98.png)
    

・sub moduleの削除
A.リポジトリとして登録済の場合

$ git submodule deinit -f [sub moduleのlocalpath]
```
$ git submodule deinit -f sub-m-test
Cleared directory 'sub-m-test'
Submodule 'sub-m-test' (https://github.com/operation-check/sub-m-test.git) unregistered for path 'sub-m-test'
```

$ git rm -f [sub moduleのlocalpath]
```
$ git rm -f sub-m-test
rm 'sub-m-test'
```  

rm -rf .git/modules/[sub moduleのlocalpath]
```
$ rm -rf .git/modules/sub-m-test

```
以上の操作でlocalから削除される。


B.リポジトリ未登録の場合
リポジトリ未登録の場合は以下の順番で削除ができる。
 （ただし実動作は未確認）
```
git reset
rm -rf .git/modules/[sub moduleのlocalpath]

# .gitmodulesを戻す
git config --local --remove-section submodule.[sub moduleのlocalpath]
git config -f .gitmodules --remove-section submodule.[sub moduleのlocalpath]

# .gitmodulesも削除する場合
rm .gitmodules
```

