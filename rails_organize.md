# Архитектура Rails-проектов

## Модели
- Модель это класс описывающий данные. в моделях содержится только объявление связей, аннотации и валидация
- Валидация, которая используется только в контексте форм, выносится в `FormObject`
- Презентационная логика выносится в  декораторы https://github.com/drapergem/draper
- Скоупы выносятся в Репозитории https://github.com/paulrayner/ddd_sample_app_ruby#repository-pattern-in-ruby http://commandercoriander.net/blog/2014/10/02/isolating-active-record/
- Бизнес логика выносится в сервис-классы 

## Контроллеры
- В контроллерах только логика `render`, `redirect_to`
- User Story выносится в Interactor'ы. можно взять [Interactor](https://github.com/collectiveidea/interactor)
- В контроллерах не юзается `helper_hethod`

## Хелперы
- хелперы не должны зависеть от моделей и прочего, только чистые функции

## View
- в Views не должны ничего знать о модели. пример: `= form_for(User.new)` здесь view знает о модели

## Сервисы

### Пример семантики сервис-классов

```ruby

class ActivateUserService

  def initialize(params)
    
    # Код, который не относится к логике домена находится в конструкторе,
    # в `replace temp with query https://sourcemaking.com/refactoring/replace-temp-with-query` и в коллбеках
    
    id =  params.fetch(:user_id)    
    @user = User.find(id)
    @param1 = params.ferch(:param1)
    @param2 = params.fetch(:param2)
  end
    
  def call
    # здесь находится доменная логика, которую легко прочитать и понять, что происходит
    if valid_user_params?
      @user.active = true
      @user.save!
    end
  end
  
  private
  
  def valid_user_params?
    if @param1 && @param2
      true
    end
  end
  
end
```

## Ссылки
- [7 Patterns to Refactor Fat ActiveRecord Models](http://blog.codeclimate.com/blog/2012/10/17/7-ways-to-decompose-fat-activerecord-models/)
- [RailsConf 2015 - What Comes After MVC](https://www.youtube.com/watch?v=uFpXKLSREQo&index=14&list=PLE7tQUdRKcybf82pLlMnPZjAMMMV5DJsK)
- [Refactoring with Hexagonal Rails](https://www.agileplannerapp.com/blog/building-agile-planner/refactoring-with-hexagonal-rails)
- [HEXAGONAL ARCHITECTURE FOR RAILS DEVELOPERS](http://victorsavkin.com/post/42542190528/hexagonal-architecture-for-rails-developers)
