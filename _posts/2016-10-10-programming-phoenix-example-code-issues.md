---
layout: post
ref: programming-phoenix-example-code-issues
date:   2016-10-10 00:00:00 +0900
title: Resolving Issues with Example Code in Programming Phoenix (Published on 2016-04-27)
lang: en
---

If you follow the Programming Phoenix book now, there are several broken example codes that will cause error, fail to pass test, or show deprecation warning. I documented such codes from Part I of the book and how to resolve them.

Those codes break because the book is written for Phoenix 1.1.x. But if you follow the installation instruction and type  

{% highlight bash %}
$ mix archive.install https://github.com/phoenixframework/archives/raw//master/phoenix_new.ez
{% endhighlight %}
as shown in the book, it will install the most recent version of Phoenix like 1.2.x.  

If you do not want to deal with this issue at all, install Phoenix 1.1.6 with this command.
{% highlight bash %}
$ mix archive.install https://github.com/phoenixframework/archives/raw//master/phoenix_new-1.1.6.ez
{% endhighlight %}

But if you want to stick to the latest version like I did, this post could be useful. But I still recommend trying to look up the changes in Phoenix yourself instead of just looking at this post, because you learn more that way. Also, it's more fun.

The authors of the book plan to update the book when Phoenix 1.3 is released, which will fix these broken examples for good.

#### Building Forms, Chapter 4 (PDF page 60)
In *rumbl/web/models/user.ex*:
{% highlight elixir %}
def changeset(model, params \\ :empty) do 
  model 
  |> cast(params, ~w(name username), []) 
  |> validate_length(:username, min: 1, max: 20) 
end 
{% endhighlight %}

Change it to

{% highlight elixir %}
def changeset(model, params \\ :empty) do
  model
  |> cast(params, [:name, :username, :password])
  |> validate_required([:name, :username])
  |> validate_length(:username, min: 1, max: 20)
end
{% endhighlight %}

cast/4 is deprecated, in favor of cast/3 + validate_required/3.  
[Ecto docs](https://hexdocs.pm/ecto/Ecto.Changeset.html#cast/4)  
[Phoenix docs](https://github.com/phoenixframework/phoenix/issues/1564)

#### Building Forms, Chapter 4 (PDF page 60-61)
In *rumbl/web/models/user.ex*:

{% highlight elixir %}
def changeset(model, params \\ :empty) do 
  model 
  |> cast(params, ~w(name username), []) 
  |> validate_length(:username, min: 1, max: 20) 
end 
{% endhighlight %}
Change it to:

{% highlight elixir %}
def changeset(model, params \\ %{}) do 
  model 
  |> cast(params, ~w(name username), []) 
  |> validate_length(:username, min: 1, max: 20) 
end 
{% endhighlight %}

`:empty` is replaced by `%{}` in Ecto 2.0. It will still work with `:empty`, though. Strangely, Video and Category models that show up in later chapters use `%{}`. 

#### Examining the Generated Controller and View, Chapter 6 (PDF page 97)
It mentions that there should be this line in *rumbl/web/controllers/video_controller.ex*:
{% highlight elixir %}
plug :scrub_params, "video" when action in [:create, :update]
{% endhighlight %}

That line is no longer automatically included in the controller to be compatible with Ecto 2.0 update.  
[Phoenix docs](https://github.com/phoenixframework/phoenix/issues/1564)

#### Testing Logged-Out Users, Chapter 8 (PDF page 135-137)
If `test "requires user authentication on all actions"` fails, you've made a similar mistake as I did. Check your *rumbl/web/router.ex* and see if it look like this.
{% highlight elixir %}
scope "/", Rumbl do
  pipe_through :browser # Use the default browser stack

  resources "/videos", VideoController
  resources "/users", UserController, only: [:index, :show, :new, :create]
  resources "/session", SessionController, only: [:new, :create, :delete]
  get "/", PageController, :index
end

scope "/manage", Rumbl do
  pipe_through [:browser, :authenticate_user]

  resources "/videos", VideoController
end
{% endhighlight %}

Delete the line `resources "videos", VideoController` in `scope "/", Rumbl do … end`. You probably saw the generation result output in Generating Resources, Chapter 6 (PDF page 92-93) and misunderstood it as the author's instruction to add the resource to *rumbl/web/router.ex*.

#### Testing Side Effect-Fere Model Code, Chapter 8 (PDF page 149-150) 
In *rumbl/test/models/user_test.exs*

{% highlight elixir %}
test "changeset does not accept long usernames" do
  attrs = Map.put(@valid_attrs, :username, String.duplicate("a", 30))
  assert {:username, {"should be at most %{count} character(s)", [count: 20]}} in 
  errors_on(%User{}, attrs)
end 
{% endhighlight %}

should be changed to

{% highlight elixir %}
test "changeset does not accept long usernames" do
  attrs = Map.put(@valid_attrs, :username, String.duplicate("a", 30))
  assert {:username, "should be at most 20 character(s)"} in 
  errors_on(%User{}, attrs)
end 
{% endhighlight %}

This issue is caused by changes to the definitino of `errors_on/2` in *rumbl/test/support/model_case.ex*.
	
#### Testing Code with Side Effects, Chapter 8 (PDF page 152)
In *rumbl/test/models/user_repo_test.exs*
{% highlight elixir %}
test "converts unique_constraint on username to error" do 
  insert_user(username: "eric")
  attrs = Map.put(@valid_attrs, :username, "eric") 
  changeset = User.changeset(%User{}, attrs) 
  assert {:error, changeset} = Repo.insert(changeset) 
  assert {:username, "has already been taken"} in changeset.errors 
end 
{% endhighlight %}

should be changed to

{% highlight elixir %}
  test "converts unique_constraint on username to error" do
    insert_user(username: "eric")
    attrs = Map.put(@valid_attrs, :username, "eric")
    changeset = User.changeset(%User{}, attrs)

    assert {:error, changeset} = Repo.insert(changeset)
    assert {:username, {"has already been taken", []}} in changeset.errors
  end
{% endhighlight %}

I think it's because of how `assert/2` works. 