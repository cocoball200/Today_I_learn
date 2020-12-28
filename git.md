git init 
git add 
git commit -m"commit message"


vim f1.txt
git commit -am "2" // add+commit 
git log  

# branch 
분기를 해서 브랜치를 만든다. 

git branch
기본 브랜치는 master 

git branch [name] 
name이라는 브랜치를 추가 

git checkout exp => exp로 이동 

ls -al 


 git log --branches 
 //모든 브랜치를 볼 수 있은 

 git log --branches --decorate

  git log --branches --decorate --graph

 git log --branches --decorate --graph --oneline

  git log exp..master 
  exp에는 없고, master에는 있는 것을 알려줌 
  git log master..exp
  master는 없고, exp에는 있는 것을 알려줌 

   git log -p  master..exp

   # 깃 병합, merge
exp에서 작업했던 커밋으로 마스터 브랜치로 합치는 것. 

git merge master 
마스터를 exp로 가져오는 것 

git branch -d exp 
exp 브랜치 삭제. 

그래서 master만 남음 

fast-forward  :빨리감기 
마스터는 hotfix를 가르키게 된다. 별도의 커밋을 생성하지 않고. 

recursive strategy 
먼저 master와 issue53의 조상을 찾고, 