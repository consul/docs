# Components

En el caso de los componentes, la personalización puede utilizarse para cambiar tanto la lógica (incluida en un archivo `.rb`) como la vista (incluida en un archivo `.erb`). Si sólo quieres personalizar la lógica, por ejemplo del componente `Admin::TableActionsComponent`, añade un archivo a `app/components/custom/admin/table_actions_component.rb` con el siguiente contenido:

```ruby
require_dependency Rails.root.join("app", "components", "admin", "table_actions_component").to_s

class Admin::TableActionsComponent
  # Su lógica personalizada aquí
end
```

Si, por el contrario, también quieres personalizar la vista, necesitas una pequeña modificación. En lugar del código anterior, utilice:

```ruby
class Admin::TableActionsComponent < ApplicationComponent; end

require_dependency Rails.root.join("app", "components", "admin", "table_actions_component").to_s

class Admin::TableActionsComponent
  # Su lógica personalizada aquí
end
```

Esto hará que el componente utilice la vista en `app/components/custom/admin/table_actions_component.html.erb`. Puedes crear este archivo y personalizarlo según tus necesidades.
