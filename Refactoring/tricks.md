## Higher Order Messages
https://laravel.com/docs/8.x/collections#higher-order-messages

#### 📛 Before
```php
static::deleting(function ($thread) {
    $thread->replies->each(function ($reply) {
        $reply->delete();
    });
});
```

#### ✔️ After
```php
static::deleting(function ($thread) {
    $thread->replies->each->delete();
});
```
