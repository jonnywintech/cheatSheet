# Laravel CheatSheet > Databaza

// umesto zagrada [   ]  uneti svoje podatke//

1. Posle instalacije Composera i Laravela /// za creiranje novog projekta

```bash
laravel new [ime_projekta]
```

2. Napraviti databazu za projekat u mysql-u

```sql
CREATE DATABASE   [database_name]       ;
```

3. Proveriti .env fajl koji sadrzi podatke o databazi na koju se konektuje
   
   .env fajl se nalazi u  folderu koji ste kreirali

4. Napraviti novi fajl koji se koristi kao template za  mysql table
   
   ```bash
   php artisan make:migration [ime_tabele]
   // primer
   php artisan make:migration create_books_table
   ```

5. Popuniti fajl  na lokaciji <mark>database/migration/[ime_fajla_bice_datum+ime__tabele]</mark>
   
   ```php
   public function up()
       {
           Schema::create('books', function (Blueprint $table) {
               $table->id();
               $table->string('title');
               $table->string('author');
               $table->timestamps();
           });
       }
   
   // function down sluzi za dropovanje databaze
    public function down()
       {
           Schema::dropIfExists('books');
       }
   ```

6. Kada se popuni fajl sa odgovarajucim parametrima izvrsiti sledecu komandu da bi se tabela dodala u databazu
   
   ```bash
   php artisan migrate
   ```

Svak-a migracija prati redosled datuma fajla I sistem prati to

A belezi u tabeli migration

## U slucaju da pullujete neciji projekat > git clone

```bash
git clone https://github.com/drstvnvc/nk16-blog.git
cd nk16-blog
composer install
cp .env.example .env
php artisan key:generate
```

popuniti .env fajl sa podacima iz databaze i onda 

```bash
php artisan migrate
php artisan serve
```

### za Pravljenje php modela

```bash
php artisan make:model Post --migration
php artisan make:model Child 
php artisan migrate:status   status migracija 
php artisan migrate:fresh   
// izvrsava reset nad databazom i ponovu upisuje iste , (samo u slucaju nuzde
)
```

komande istraziti

@extends

@yield

@include

@section

@endsection
