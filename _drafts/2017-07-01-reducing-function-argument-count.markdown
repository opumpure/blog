---
layout: post
title: "Reducing Function Argument Count"
date: "2017-07-01 18:13:30 +0300"
tags: programming quality oop
affiliate_url: http://amzn.to/2un9mMD
---

If you read books like [Clean Code]({{ page.affiliate_url }}) you would have come across recommendations for function parameter count. Basically, it says that the more arguments the more questioning you should be about your code design.

A function should preferably have no arguments (niladic). Then the recommendations go on and list a **few** cases where **one** function argument (monadic) is **OK** to use. Even fewer justifications for two arguments (diadic) and so on.

When I first came across this, I really liked the idea. However, I didn't understand how to practically apply it to my daily coding routine. As if I realized the cause that my code was suffering from, but din't know how to improve it. Because when I looked at my code it had patterns like this:

```
class UserRepository
{
    public function getAllByCompany(
        $companyId,
        $type,
        $isActive
    ) {
        // SQL query
    }
}
```

We can improve the function by extracting the `$type` and `$isActive` arguments into different functions. But what if I wanted to get rid of `$companyId` too?

This is a very basic example and you could argue that having the company identifier there is justifiable. However, for the sake of this discussion, lets say that we want to make the function niladic.

## The Answer

Constructors.

```
class UserRepository
{
    private $companyId;
    public function __construct($companyId)
    {
        $this->companyId = $companyId;
    }
    public function getAllActiveAdmins()
    {
        // SQL query
    }
}
```

User repository takes the company identifier as a constructor argument. Languages like Java allow constructor overloading, which would make the solution even cleaner.

The recommendation to reduce function arguments has made me reconsider my design and potential improvements. Programmer's thinking changes to a more object oriented style, because the class is initialized with a state. User repository usage in code is cleaner and more readable. Initialize the class once and retrieve the users afterwards, without dragging the company identifier along.
