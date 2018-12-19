# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...

## 追加するgem
```
gem 'cocoon'
gem 'jquery-rails'
gem 'jquery-ui-rails'
```

## application.jsに追加
```
//= require turbolinks
//= require jquery  <=---ここ
//= require cocoon  <=---ここ
//= require_tree .
```

## モデルに追記
#### app/models/project.rb
```
  class Project < ApplicationRecord
    has_many :tasks
    accepts_nested_attributes_for :tasks, allow_destroy: true  # 削除も受け入れる
  end
```
#### app/models/task.rb
```
  class Task < ApplicationRecord
    belongs_to :project
    validates :name, presence: true
  end
```

## コントローラに追記
#### app/controller/projects_controller.rb
```

  private

  def order_params
    params.require(:order).permit(:title, :customer, order_details_attributes: [:id, :_destroy, :item, :price, :quantity])
  end
```

## ビューに追記
#### 親側の入力フォーム(app/views/projects/_form.html.erb)
```
...
  <div class="details">
    <%= link_to_add_association '行を追加', form, :tasks %>
    <table>
      <%= form.fields_for :tasks do |task| %>
        <%= render 'task_fields', f: task %>
      <% end %>
    </table>
  </div>
...
```

#### 子側の入力フォーム(app/views/orders/_form.html.erb)
```
  <tr class="nested-fields">
    <td><%= f.text_field :name %></td>
    <td><%= link_to_remove_association "行削除", f %></td>
  </tr>
```

#### 参考文献
* 「nathanvda/cocoon」 ( https://github.com/nathanvda/cocoon#partial )
* 「Rails5とcocoonを使って明細のあるフォームを作る」 ( https://twinbird-htn.hatenablog.com/entry/2018/05/17/230000 )