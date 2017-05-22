--- 
wordpress_id: PSD-19
layout: post
title: DatabaseCleaner and include_context
---

I work on a legacy product. It's lovely 'n all, but it does have a pretty slow database suite. Sure, a lot of that is my fault, but not _all_ of it ;) Anyway, one of the things that this app currently needs is `fixtures`. Fixtures aren't great as they are basically saying "my app needs this particular setup to work", but, hey, that's the way it is.

Now, it's best if you can run your test suite using `transactions`, as it's fast. However, if are running tests in the browser (e.g. I click on this button and stuff happens) then you need to use `truncation` for those tests.

So the issue I was having is that I had a test which looks like this:

```ruby
  it "Doesn't add if the problem is already part of the document" do
    expect {
      post :create, document_id: existing_document.hashed_id, problem_ids: "#{Problem.all.map(&:id).join(",")}"
    }.not_to change { DocumentProblem.count }
  end
```

This is actually a bad test as we really shouldn't be using the `Problem.all.map` rather using what is in the document already, but it threw up the issue. The problem was that there were `Problems` from another spec in the database, from a spec which had used `fixtures`. In another spec I had:


```ruby

# some other spec
context "with problems" do
  include_context "common export setup"

# the context file

shared_context "common export setup" do
  fixtures :documents, :styles, :problems, :problem_links, :document_problems, :users, :public_codes, :scoring_app_infos
```

I had [previously read a very helpful article](http://tomdallimore.com/blog/taking-the-test-trash-out-with-databasecleaner-and-rspec/) which pointed me to the issue. In my database cleaner I had this:

```ruby
  config.before(:each) do
    DatabaseCleaner.strategy = :transaction
  end
```

But the problem _is that the fixtures are NOT being loaded in an `each` block_ so DatabaseCleaner doens't pick them up.

Solution for me, move my fixtures to be all global so I can continue to use transactions. Not really what I want to do, and maybe there's a better way (please let me know if there is!) but it'll do for the moment.
