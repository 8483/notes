# Bulk insert

Fastest, but has a limit of 1,000 inserts.

```js
await request.query(`
    insert into table 
        (column1, column2, column3)
    values 
        ('foo', 'bar', 'baz'),
        ('foo', 'bar', 'baz'),
        ('foo', 'bar', 'baz')
    ;
`);
```

Best

```js
await request.query(`
    insert into table 
        (column1, column2, column3)
    values 
        ('foo', 'bar', 'baz');

    insert into table 
        (column1, column2, column3)
    values 
        ('foo', 'bar', 'baz');

    insert into table 
        (column1, column2, column3)
    values 
        ('foo', 'bar', 'baz');
`);
```

Slowest

```js
await request.query(`
    insert into table 
        (column1, column2, column3)
    values 
        ('foo', 'bar', 'baz');
`);

await request.query(`
    insert into table 
        (column1, column2, column3)
    values 
        ('foo', 'bar', 'baz');
`);

await request.query(`
    insert into table 
        (column1, column2, column3)
    values 
        ('foo', 'bar', 'baz');
`);
```
