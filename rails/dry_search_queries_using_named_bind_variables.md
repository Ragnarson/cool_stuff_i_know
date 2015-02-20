# DRY search queries using named bind variables

Sometimes we need to search various columns in a database for a particular term. Our query might look something like this:

```
User.where("field1 = ? OR field2 = ? OR field3 = ?", email, email, email)
```

It's not very DRY, but fortunatelly there is a better way to achieve this.
We can use named bind variables, which allows you to replace the question mark 
placeholders in the conditions array with a symbol. The values corresponding to these symbol
placeholders can be then accessed via a hash.

We can refactor our User search:

```
User.where("field1 = :email OR field2 = :email OR field3 = :email", :email => email)
```

