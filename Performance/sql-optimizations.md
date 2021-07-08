# 🔥 N+1 select problem 🚒
```php
class Thread {
  public function link()
  {
      return "/threads/{$this->channel->slug}/{$this->id}";
  }
}
```
### 📛 Channels are lazy-loaded
![image](https://user-images.githubusercontent.com/28957748/124854645-d5058600-dfd1-11eb-8c15-5767881abd5b.png)

### ✔️ Eager-load channels
```php
$threads = Thread::with('channel')->latest()->filter($filters);
```
![image](https://user-images.githubusercontent.com/28957748/124855804-b2746c80-dfd3-11eb-907d-4b45c13c7e90.png)


# 🔥 Multiple queries on unchanged resource 🚒
### 📛 Spot the problem 👀
![image](https://user-images.githubusercontent.com/28957748/124857398-6c6cd800-dfd6-11eb-9e57-bd2aaaa89a80.png)

```php
<?php
class AppServiceProvider extends ServiceProvider
{
    public function boot()
    {
        View::composer('*', function ($view) {
            $view->with('channels', Channel::all());
        });
    }
}
```
### ✔️ Query for the first time and pull from cache since then
```php
View::composer('*', function ($view) {
    $channels = Cache::rememberForever('channels', function () {
        return Channel::all();
    });

    $view->with('channels', $channels);
});
```
