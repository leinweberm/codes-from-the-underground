# USING in TypeScript 5.2

``` js
const getResource = () => {
  return {
    [Symbol.dispose]: () => {
      console.log('Hooray!')
    }
  }

  using resource = getResource()
}
```

Vše uvnitř scopu funkce getResource může být zničeno/dispose.

Lze použít i s async funkcí:
``` js
const getResource = () => ({
  [Symbol.asyncDispose]: async () => {
    await someAsyncFunc();
  },
});

await using resource = getResource();
```

Ukázka použití v realném světě:
``` js
const getConnection = async () => {
  const connection = await getDb();

  return {
    connection,
    [Symbol.asyncDispose]: async () => {
      await connection.close();
    }
  }
};

await using { connection } = getConnection();
// Do stuff with connection
// Automatically closed!
```
V této ukáce si vytvořítě nové spojení s databázi. Vykonáte nad ním nějaké operace a nezávisle na tom zda se operace podaří nebo nebo se následně spojení do databáze samo zavře.
