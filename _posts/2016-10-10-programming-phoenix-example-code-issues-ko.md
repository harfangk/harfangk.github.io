---
layout: post
ref: programming-phoenix-example-code-issues
date:   2016-10-10 00:00:00 +0900
title: Programming Phoenix (2016-04-27 출판본)의 문제 있는 예제 코드 모음
lang: ko
---

지금 시점에서 Programming Phoenix 책에 있는 예제 코드를 따라해보면 일부 코드에서 문제가 발생합니다. 에러가 나기도 하고, 테스트를 통과하지 않을 때도 있고, 데프러케이션 경고가 뜨기도 합니다. 해당 책의 Part I에서 문제가 발생하는 예제 코드를 모아보고, 그 해결책을 정리했습니다.

예제 코드에서 문제가 발생하는 이유는 지금 책이 Phoenix 1.1.x 버전을 바탕으로 쓰였기 때문입니다. 하지만 책에 나온 Phoenix 설치 방법을 따라서

{% highlight bash %}
$ mix archive.install https://github.com/phoenixframework/archives/raw//master/phoenix_new.ez
{% endhighlight %}
라고 타이핑하면 1.2.x 버전의 최신 Phoenix가 설치됩니다.

예제 코드에 문제가 발생하는 것 자체를 피하고 싶으시면 그냥 아래 커맨드를 사용해서 Phoenix 1.1.6을 설치하시면 됩니다.
{% highlight bash %}
$ mix archive.install https://github.com/phoenixframework/archives/raw//master/phoenix_new-1.1.6.ez
{% endhighlight %}

하지만 저처럼 최신 버전을 사용하는 것을 선호하신다면 문제가 있을 때마다 이 글을 참고하는게 도움이 될 겁니다. 물론 Phoenix 프레임워크의 변경점을 직접 찾아보시는 편이 배우기에도 더 좋은만큼 스스로 찾아보시는 것을 더 추천합니다. 별로 찾기 어렵지 않거든요.

저자들의 말에 따르면 Phoenix 1.3이 출시되면 그에 맞춰서 책을 개정한다고 하니 그 이후로는 예제 코드 문제가 없어질 것 같습니다.

#### Building Forms, Chapter 4 (PDF page 60)
*rumbl/web/models/user.ex*:
{% highlight elixir %}
def changeset(model, params \\ :empty) do 
  model 
  |> cast(params, ~w(name username), []) 
  |> validate_length(:username, min: 1, max: 20) 
end 
{% endhighlight %}

위 코드를 아래와 같이 변경해주세요.

{% highlight elixir %}
def changeset(model, params \\ :empty) do
  model
  |> cast(params, [:name, :username, :password])
  |> validate_required([:name, :username])
  |> validate_length(:username, min: 1, max: 20)
end
{% endhighlight %}

`cast/4`는 더 이상 사용하지 않고` cast/3` + `validate_required/3`로 대체되었습니다..  
[Ecto 문서](https://hexdocs.pm/ecto/Ecto.Changeset.html#cast/4)  
[Phoenix 문서](https://github.com/phoenixframework/phoenix/issues/1564)

#### Building Forms, Chapter 4 (PDF page 60-61)
*rumbl/web/models/user.ex*:

{% highlight elixir %}
def changeset(model, params \\ :empty) do 
  model 
  |> cast(params, ~w(name username), []) 
  |> validate_length(:username, min: 1, max: 20) 
end 
{% endhighlight %}

위 코드를 아래와 같이 변경해주세요.

{% highlight elixir %}
def changeset(model, params \\ %{}) do 
  model 
  |> cast(params, ~w(name username), []) 
  |> validate_length(:username, min: 1, max: 20) 
end 
{% endhighlight %}

Ecto 2.0부터 `:empty`가 `%{}`로 대체되었습니다.. `:empty`를 사용해도 아직 작동은 하더군요.. 왠지 모르겠지만 책 뒷부분에 나오는 Video와 Category 모델에는 저자들이 `%{}`를 사용했습니다.. 

#### Examining the Generated Controller and View, Chapter 6 (PDF page 97)
책을 보면 *rumbl/web/controllers/video_controller.ex* 파일에 아래와 같은 내용이 있을 거라고 되어있습니다.
{% highlight elixir %}
plug :scrub_params, "video" when action in [:create, :update]
{% endhighlight %}

저 코드는 Ecto 2.0 업데이트 때문에 더 이상 컨트롤러 안에 자동으로 추가되지 않도록 바뀌었습니다. 
[Phoenix 문서](https://github.com/phoenixframework/phoenix/issues/1564)

#### Testing Logged-Out Users, Chapter 8 (PDF page 135-137)
`test "requires user authentication on all actions"`가 실패한다면 저와 비슷한 실수를 해서 그럴 겁니다. *rumbl/web/router.ex* 파일을 확인해보시고, 아래와 같이 되어 있나 살펴보세요.
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

`scope "/", Rumbl do … end` 부분에 있는 `resources "videos", VideoController` 줄을 삭제해주세요. Generating Resources, Chapter 6 (PDF page 92-93)에 나온 제너레이터 결과 텍스트를 보시고 저자가 video 리소스를 *rumbl/web/router.ex*에 추가하라고 말한 것이라고 오해해서 발생한 문제입니다.

#### Testing Side Effect-Fere Model Code, Chapter 8 (PDF page 149-150) 
In *rumbl/test/models/user_test.exs*

{% highlight elixir %}
test "changeset does not accept long usernames" do
  attrs = Map.put(@valid_attrs, :username, String.duplicate("a", 30))
  assert {:username, {"should be at most %{count} character(s)", [count: 20]}} in 
  errors_on(%User{}, attrs)
end 
{% endhighlight %}

위 코드를 아래와 같이 변경해주세요.

{% highlight elixir %}
test "changeset does not accept long usernames" do
  attrs = Map.put(@valid_attrs, :username, String.duplicate("a", 30))
  assert {:username, "should be at most 20 character(s)"} in 
  errors_on(%User{}, attrs)
end 
{% endhighlight %}

*rumbl/test/support/model_case.ex*에 정의되어 있는 `errors_on/2` 함수가 바뀌어서 발생한 문제입니다. 
	
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

위 코드를 아래와 같이 변경해주세요.

{% highlight elixir %}
  test "converts unique_constraint on username to error" do
    insert_user(username: "eric")
    attrs = Map.put(@valid_attrs, :username, "eric")
    changeset = User.changeset(%User{}, attrs)

    assert {:error, changeset} = Repo.insert(changeset)
    assert {:username, {"has already been taken", []}} in changeset.errors
  end
{% endhighlight %}

`assert/2` 함수가 작동하는 방식이 바뀐 것 같습니다.