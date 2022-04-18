# Laravel CheatSheet > API

Preimenovanje ili Konvertovanje jednog tipa podataka u drugi, string u boolean ili data  u zavisnosti sta je potrebno

Dodaje se u modelu vezanog za databazu u kome se menja

```php
 protected $casts = [
        'is_automatic' => 'boolean',
    ];
```

Pravljenje Controlera kada je u pitanju api sa vec unetim klasama  kada se iniciativno postavi

```bash
php artisan make:controller --api --model=Car CarController
```

Pravljenje Requestova prema api

```bash
php artisan make:request CreateCarRequest #UpdateCarRequest
```

Unutar request-a definisemo koja polja su required i njihove parametre

```php
<?php

namespace App\Http\Requests;

use App\Models\Car;
use Illuminate\Foundation\Http\FormRequest;

class CreateCarRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            'brand' => 'required|string|min:2',
            'model' => 'required|string|min:2',
            'year' => 'required|integer|between:1990,2022',
            'max_speed' => 'integer|between:20,300',
            'is_automatic' => 'required|boolean',
            'engine' => 'required|string|in:' . implode(',', Car::ENGINES),
            'number_of_door' => 'required|integer|between:2,5',
        ];
    }
}
```

Update izgleda ovako

```php
<?php

namespace App\Http\Requests;

use App\Models\Car;
use Illuminate\Foundation\Http\FormRequest;

class UpdateCarRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            'brand' => 'string|min:2',
            'model' => 'string|min:2',
            'year' => 'integer|between:1990,2022',
            'max_speed' => 'integer|between:20,300',
            'is_automatic' => 'boolean',
            'engine' => 'string|in:' . implode(',', Car::ENGINES),
            'number_of_door' => 'integer|between:2,5',
        ];
    }
}
```

Controller Klasa izgleda ovako

```php
  // show funkcija koja dobavlja sve podatke
    public function index()
    {
        $cars = Car::all();
        return response()->json($cars);
    }

  // sacuvavanje podataka
    public function store(CreateCarRequest $request)
    {
        $data = $request->validated();
        $car = Car::create($data);
        return response()->json($car);
    }     
  // prikazivanje samo jednog auta
    public function show(Car $car)
    {
        return response()->json($car);
    }
  // update  nekih ili svih atributa auta
    public function update(UpdateCarRequest $request, Car $car)
    {
        $car->update($request->validated());
        return response()->json($car);
    } 
  // brisanje podataka
   public function destroy(Car $car)
    {
        $car->delete();
        return response(null, 204);
    }
```

Paginacija Po stranici -> umesto $car=Car::all() -> ide sledeca komanda

Nalazi se u Kontroleru

```php
   public function index(Request $request)
    {
        $pageSize = $request->query('per_page', 10);
        $brand = $request->query('brand');
        $model = $request->query('model');

        $query = Car::query();

        if ($brand) {
            $query->searchByBrand($brand);
        }
        if ($model) {
            $query->searchByModel($model);
        }

        $cars = $query->paginate($pageSize);

        return response()->json($cars);
    }
```

Pretraga po querrybuilderu u gore napisanoj paginaciji

Nalazi se u modelu Klase

```php
 public function scopeSearchByBrand($query, $brand)
    {
        $query->where('brand', 'like', "%$brand%");
    }

    public function scopeSearchByModel($query, $model)
    {
        $query->where('model', 'like', "%$model%");
    }
```
