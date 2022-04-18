Za kreiranje posta dodajemo novu metodu u PostController-u

```php
use Illuminate\Support\Facades\DB;

 public function create(Request $request)
    {
        return view('create-post');
    }

    public function store(Request $request)
    {
        DB::listen(function ($query) {
            info($query->sql);
        });

        // $post = new Post();
        // $post->title = $request->get('title', '');
        // $post->body = $request->get('body', '');
        // $post->is_published = $request->get('is_published', false);
        // $post->save();

        // $data = [
        //     'title' => $request->get('title', ''),
        //     'body' => $request->get('body', ''),
        //     'is_published' => $request->get('is_published', false),
        // ];
        $data = $request->only(['title', 'body', 'is_published']);

        $post = Post::create($data);

        // return view('post', compact('post'));
        return redirect("/posts/$post->id");
    }
```

u routes  Web.php  dodajemo  dve metode 

```php
Route::get('/posts/create', [PostController::class, 'create']);

Route::post('/posts', [PostController::class, 'store']);
```
