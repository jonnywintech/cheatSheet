git config --global user.name 'Ime Prezime'

git config --global user.email 'moj_email@gmail.com'

!!!!!!!!tekst u zagradama i zagrade izbrisati  dodati odgovarajuce!!!!!!!



U SLUCAJU DA JE NAPRAVLJEN GIT REPO 

///////////////////
1 markiranje direktorijuma za git upload kada vi pravite nov projekat
//////////////////
git init
//////////////////


u slucaju da se prikljucujete na neciji projekat i nastavljate sa radom
git clone (link do stranice)


/////////////////
2 Za dodavanje svih file-ova u staging area
/////////////////
git add -A
//////////////////
2.1 Za reset Dodavanja fajlova greskom
/////////////////
git reset <file>  
////////////////


/////////////////
3 za proveru trenutnog statusa fajlova
/////////////////
git status
//////////////////


/////////////////
4 Za pakovanje na lokalnom racunaru
/////////////////
git commit -m '(Naziv poruke, sta je promenjeno itd)'
//////////////////
4.1 Za reset svih comitova koji nisu pushovani na github i vracanje na zadnji koji je pushovan
/////////////////
git stash (proveriti sledecu komandu u skladu sa dokumentacijom online)
////////////////


/////////////////
5 Za upload na server najpre selektovati link do odgovarajuceg repo-a
/////////////////
git remote add origin (primer zameniti https://github.com/shiftshader/sdaswdawd.git)
//////////////////


/////////////////
6 za selektovanje brancha na koji se uploaduje
/////////////////
git checkout (ime brancha)
//////////////////


/////////////////
7 za upload na git server
/////////////////
git push origin master
//////////////////


ali u slucaju da radite u  timu programera potrebno je izvrsiti komandu:
//////////////////
git pull origin master ( u slucaju da radite  na drugoj grani unesite ime grane)
//////////////////
pre svakog push-a radi provere da ne bi doslo do nekog preklapanja koda




/////////////////
8 za pravljenje nove grane tj branch-a
/////////////////
git branch (ime branch-a)
//////////////////



/////////////////
9 za promenu trenutnog branch-a
/////////////////
git checkout (ime branch-a)
//////////////////



/////////////////
10 za spajanje grane na master prebaciti se na master sa checkout komandom i:
/////////////////
git merge (ime branch-a sa kojim se spaja trenutna) 
//////////////////


/////////////////
11 ignorisanje fajlova i direktorijuma se moze izvrsiti kreiranjem fajla:
/////////////////
.gitignore
//////////////////
gitignore je obican text dokument i u njemu se smestaju imena i tip fajla ili direktorijuma 
koji ce biti izuzeti od git protokoli i nece biti uploadovani na server


!!!!!!!!!!!!!!!!! new !!!!!!!!!!!!!!

//////////////////
git log  
//////////////////
loguje sve comitove  vezane za projekat
//////////////////



//////////////////
git commit  --amend -m "update message about commit"
//////////////////
{za izmenu poruka o commitu}
//////////////////


//////////////////
git reflog
//////////////////
{pregled  updejta vezana za commit koji smo updajtovali
ukljucujuci i amend}
//////////////////


//////////////////
Brisanje brancha
//////////////////
git branch -d (ime branch-a)
//////////////////


//////////////////
kreiranje brancha i prelazak na taj branch
//////////////////
git checkout -b (ime branch-a)
//////////////////

