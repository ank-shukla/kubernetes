git init

git status --ignored

git add .

git commit

git remote add origin https://github.com/ankshukz/kubernetes.git
git remote set-url origin https://github.com/ank-shukla/kubernetes.git

git config --global user.email "ankitshukla2806@gmail.com"
git config --global user.name "Ankit Shukla"

git commit -m "First Commit"

git push -u origin master