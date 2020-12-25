Sqlite in memory test database
==============================

Helpers to create an in-memory Sqlite database
----------------------------------------------

The sqliteInMemory.CreateOptions<T> method will create options that will provide a sqlite, in-memory database for unit testing. The code below shows how is can be used.

``` csharp
[Fact]
public void TestSqliteOk()
{
    //SETUP
    var options = SqliteInMemory.CreateOptions<EfCoreContext>(); 
    using (var context = new EfCoreContext(options))
    {
        //... rest of unit test goes here
```

EF Core 3+ ONLY
^^^^^^^^^^^^^^^

There is a optional parameter that allows you to set extra options and/or override given options in the DbContextOptionsBuilder<T> level. Below is part of the unit tests showing how to add/override options.

``` csharp
//... previous code removed to focus on this feature
var options2 = SqliteInMemory.CreateOptions<BookContext>(builder =>
{
    builder.UseSqlite(connection); //overrides current connection with existing connection
    builder.UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking); //change track option
});
using (var context = new BookContext(options2))
{
    //VERIFY
    var book = context.Books.First();
    context.Entry(book).State.ShouldEqual(EntityState.Detached);
}
```