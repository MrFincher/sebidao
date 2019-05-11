# sebidao
Opinionated and generic data access object implementation for postgreSQL database.

It does not use postgreSQL features per se, but it is only tested with postgreSQL.

This DAO is a work in progress.
It supports the basic CRUD operations in a generic withn little help
of the user-programmer.

The only required helper class to an entity say student is any mapper, like StudentMapper,
which knows enough of the student class to take a student instance apart in an array
of objects and do the reverse.
The Student class would probably be friendly and lend a helping hand to the Mapper by providing just such methods, but only package private, mind you. That will do the work.

The javadoc can be found at the [Fontys Venlo Javabits website](https://javabits.fontysvenlo.org/) under
[Sebi DAO javadoc](https://javabits.fontysvenlo.org/sebidao/apidocs/index.html)

## The TAO of DAO

A DAO should be simple to use. Adding complexity that serves a niche use case but
increases complexity for the simple use cases is contrary to the [TAO](https://en.wikipedia.org/wiki/Tao) of DAO.

DAO is a pattern to reduce coupling. This implies that the application logic should
not know not care what the underlying persistence technology is. The better this objective
is served, the more reuse can be achieved in the only valuable part of the software: the application logic it selves.

The current implementation supports the following operations:

```Java

public interface DAO<K extends Serializable, E extends Entity2<K>> extends AutoCloseable {
    Optional<E> get( K id );
    Collection<E> getAll();
    Collection<E> getByColumnValues( Object... keyValues );
    E save( E e );
    E update( E e );
    void delete( E e );
    void delete( K k );
    TransactionToken startTransaction() throws Exception;
    DAO<K, E> setTransactionToken( TransactionToken tok );
    TransactionToken getTransactionToken();
    int size();
    int lastId();
    void close() throws Exception;
    Collection<? extends E> saveAll( Iterable<E> entities );
    Collection<? extends E> saveAll( E... entities );
    void deleteAll( Iterable<E> entities );
    void deleteAll( E... entities );
}

```

The [Javadoc](https://javabits.fontysvenlo.org/sebidao/apidocs/index.html) of the development branch is hosted at [javabits.fontysvenlo.org](https://javabits.fontysvenlo.org/)
