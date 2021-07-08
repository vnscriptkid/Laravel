# Use case 1: Only thread's owner can delete his own threads
### Simple way
```php
// In controller
if (auth()->id() !== $thread->owner->id) {
    abort(403, 'Permission denied');
}
```

### Complex way
- Make policy
```console
php artisan make:policy ThreadPolicy --model=Thread
```

- Implement policy
```php
public function update(User $user, Thread $thread)
{
    return $user->id === $thread->owner->id;
}
```

- Register policy
```php
// In AuthServiceProvider
protected $policies = [
    'App\Models\Thread' => 'App\Policies\ThreadPolicy',
];
```

# Use case 2: Admin can do anything
### In scope of a specific resource 
```php
// ThreadPolicy
public function before($user)
{
    if ($user->email === 'mod@gmail.com') {
        return true;
    }
}
```

### Absolute anything
```php
// AuthServiceProvider
public function boot()
{
    $this->registerPolicies();
    
    Gate::before(function ($user) {
        if ($user->email === 'admin@gmail.com') return true;
    });
}
```

