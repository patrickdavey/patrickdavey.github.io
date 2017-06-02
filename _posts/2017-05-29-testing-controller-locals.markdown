--- 
wordpress_id: PSD-20
layout: post
title: Testing controller locals
---

I have taken to trying to explicitly calling controller actions passing in the locals, rather than using `@ivars` and allowing them to be magically passed through to the view.

In a test I wanted to check that the presenter was assigned correctly as a local, and that it had the correct variable assigned to it.

With help from [this stackoverflow](https://stackoverflow.com/a/37364871)
```ruby
    before do
      sign_in user
      allow(controller).to receive(:render).and_call_original
    end

    expect(controller).to have_received(:render) do |_method, options|
      expect(options[:locals][:presenter].something.count).to eq(1)
    end
```
