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
   php artisan make:migration [ime_tabele] ## create_user_table
   ## primer
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
               $table->foreignId('user_id')
               ->constrained()
               ->onDelete('cascade');
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
   php artisan migrate:fresh ##brise sve tabele i ponovo ih postavlja
   php artisan migrate:rollback #vraca tabele u predhodno stanje migracij
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

u .env fajlu u slucaju da se radi u produkciji promeniti APP_ENV= production

u ovom slucaju daje upozorenje  u slucaju da slucajno ukucate komande koje 

mogu da izbrisu databaze ili tabele i time obrisete podatke

## za Pravljenje php modela i controlera

```bash
php artisan make:model Post #--migration
php artisan make:controller [imeControlora] #pravi kontroler fajl !!

php artisan migrate:status  #status migracija 
php artisan migrate:fresh #izvrsava reset nad databazom 
#i ponovu upisuje iste , (samo u slucaju nuzde)!!!
```

Podesavanja u Controlleru app/Http/Controllers

```php
namespace App\Http\Controllers;
use App\Models\Post; // ime Post zavisi od projekta do projekta i moze se menjati
use Illuminate\Http\Request;
class PostController extends Controller
{
    public function index()
    {
        $posts = Post::where('is_published', true)->get();
        return view('posts', compact('posts')); // [ 'posts' => $posts]
    }
    public function show($id)
    {
        // $post = Post::where('id', $id)->first();
        $post = Post::findOrFail($id);
        return view('post', compact('post')); // [ 'post' => $post]
    }
}
```

Ove dve funkcije se posle toga pozivaju u routes/web.php

```php
use App\Http\Controllers\PostController;
use Illuminate\Support\Facades\Route;
/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/
Route::get('/posts', [PostController::class, 'index']);
Route::get('/posts/{id}', [PostController::class, 'show']);
```

Pravljenje  stranice iz delova tako da sadrzaj bude dinamican i nema previse ponavljanja  u folderu  /resources/views prave se dva foldera  'layouts' i 'partials'

U layoutu se nalazi glavna sekcija webstranice head i   telo koje sadrzi 'partials'

U partials se nalaze delovi stranice koji male sekcije koje se  @include u layouts

i uglavnom su header(navbar) i footer

Komande  u stranicama

@include  ---  deo stranice koji se prilkjucuje glavnom sadrzaju (navbar / footer)

```php
 @include('partials.navbar')
```

@extends ---  iskoristi  napravljeni template  (header/footer)

```php
@extends('layouts.app')
```

@yield -- nalazi se u delu stranice gde su dinamicni podaci  i pozivaju se 

sa @section u novoj stranici

```php
<title>@yield('title')</title>
// onda ide druga stranica u kojoj je @include i onda -->
```

stranica na kojoj se poziva @yeld

```php
@section('title', 'Vivify Blog')
//primer 2
@section('content')
<h1>Posts</h1>
<ul>
    @foreach($posts as $post)
    <li>
        <a href="/posts/{{$post->id}}">{{$post->title}}</a>
    </li>
    @endforeach
</ul>
@endsection
```

Veze izmedju Tabela

1. hasone

2. hasMany

3. belongsTo

4. belongsToMany

ovo se izgugla ili istrazi eloquent relationship u laravel dokumentaciji

#### Relacija se pravi u Model folderu i pise se ovako

```php
public function team()
    {
        return $this->belongsTo(Team::class);
    }
```

ovo je  u  modelu igrac 

dalje se pise u kontroleru 

```php
 public function show($id)
  {
      $team = Teams::with('players')->findOrFail($id);//::with je relacija
 //izmedju databaza i smesta se u team variablu koja se prosledjue u view
    //   dd($team);
      return view('pages.team', compact('team'));
  }
```

random copy

```bash

```
