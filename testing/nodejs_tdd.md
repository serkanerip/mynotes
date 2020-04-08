# NodeJS Ile TDD


![](https://source.unsplash.com/1600x900/?coding)

Jest kütüphanesi ile Nodejs'de TDD uygulayarak basit bir uygulama yapacağız.

Bu makalede yapılan uygulamanın tamamlanmış haline bu [link](https://github.com/serkanerip/TDD_Example)'ten ulaşabilirsiniz.

Yapacağımız uygulama:

- PostgreSQL üzerinde users adlı bir tablomuz olacak.
- PG modülünü kullanarak, CRUD işlemları yapan bir uygulama yazacağız.

İhtiyaçlarımız:

- UserRepository: Veritabanındaki kullanıcı tablosu üzerinde işlemler yapacak olan katman.
- UserService: Client ile repository'i birbirine bağlayan ve aradaki logic işlemleri yapan katman.
- User: Veritabanındaki users tablomuza denk gelen js objemiz.

Boilerplate:

```console
foo@bar:~$ mkdir testing-kata
foo@bar:~$ cd testing-kata
foo@bar:~$ npm init -y
foo@bar:~$ npm install jest --save-dev
foo@bar:~$ npm install pg --save
foo@bar:~$ touch index.js
foo@bar:~$ mkdir src __test__
```

Proje yapımızı oluşturan kodları oluşturduktan sonra test yazmaya başlıyoruz.

```console
foo@bar:~$ touch User.test.js UserRepository.test.js UserService.test.js
```

Öncelikle User modelimiz için test yazıcağız.

**User.test.js**:

```js
const User = require("../src/model/User");

describe("User Model Tests", () => {
  it("SHOULD_HAVE_USERNAME_NAME_AND_PASSWORD_KEYS", () => {
    const testUser = User();
    expect(testUser).toHaveProperty("name");
    expect(testUser).toHaveProperty("userName");
    expect(testUser).toHaveProperty("password");
  });
});
```

Şimdi testimizi çalıştıralım.

```bash
./node_modules/.bin/jest __test__/User.test.js --watch
```

```bash
FAIL  __test__/User.test.js
  User Model Tests
    ✕ SHOULD_HAVE_USERNAME_AND_PASSWORD_KEYS (1ms)

  ● User Model Tests › SHOULD_HAVE_USERNAME_AND_PASSWORD_KEYS

    TypeError: User is not a function

      3 | describe("User Model Tests", () => {
      4 |   it("SHOULD_HAVE_USERNAME_AND_PASSWORD_KEYS", () => {
    > 5 |     const testUser = User();
        |                      ^
      6 |     expect(testUser).toHaveProperty("name");
      7 |     expect(testUser).toHaveProperty("userName");
      8 |     expect(testUser).toHaveProperty("password");

      at Object.it (__test__/User.test.js:5:22)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 1 total
Snapshots:   0 total
Time:        0.346s, estimated 1s
```

Testimiz beklediğimiz gibi fail oldu şimdi testimizin geçmesi için kodumuzu yazıyoruz. :collision:

**/src/model/User.js**:

```js
const User = () => {
  return { name: "", userName: "", password: "" };
};

module.exports = User;
```

```bash
 PASS  __test__/User.test.js
  User Model Tests
    ✓ SHOULD_HAVE_USERNAME_AND_PASSWORD_KEYS (4ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.452s, estimated 1s
Ran all test suites matching /__test__\/User.test.js/i.
```

Şimdi testimizin başarıyla geçtiğini görüyoruz, User modelimiz için bu kadar test yeterli.

UserRepository için şimdi test yazıcağız.
UserRepository postgresql işlemleri için pg modülünü kullanacak. Unit test yazdığımız için testimizin pg modülünü de kapsamaması için pg modülünü mocklamamız gerekiyor.

**UserRepository.test.js**:

```js
const { Pool } = require("pg");

jest.mock("pg", () => {
  return {
    Pool: jest.fn(() => {
      return {
        connect: jest.fn(() => {
          return {
            query: jest.fn(() => {
              return { rows: [] };
            }),
            release: jest.fn(),
          };
        }),
      };
    }),
  };
});
```

Pg modülünün Pool nesnesi bir constructor function ve bu da query, release ve daha bir çok metod geri döndermektedir.
Bizim ihtiyacımız olanlar şuanda query ve release o yüzden bunlarıda mockluyoruz.

Şimdi repositoryimizde kullanacağımız metodları belirleyelim:

- getAll
- find
- create
- deleteUser -> delete keywordü js tarafından kullanıldığı için kullanamıyoruz.
- update

Şimdi bu fonksiyonlar için gerekli testlerimizi yazalım.

**UserRepository.test.js**:

```js
const { Pool } = require("pg");
const userRepository = require("../src/repository/UserRepository");

jest.mock("pg", () => {
  return {
    Pool: jest.fn(() => {
      return {
        connect: jest.fn(() => {
          return {
            query: jest.fn(() => {
              return { rows: [] };
            }),
            release: jest.fn(),
          };
        }),
      };
    }),
  };
});

let pool, client;

describe("User Repository Tests", () => {
  beforeEach(async () => {
    pool = new Pool();
    client = await pool.connect();
    client.query.mockResolvedValueOnce({ rows: [], rowCount: 0 });
  });

  test("should give all users as array", async () => {
    const dbResult = await userRepository.getAll(client);
    expect(client.query).toBeCalledTimes(1);
    expect(client.query).toBeCalledWith("SELECT * FROM users");
    expect(dbResult).toHaveProperty("rows");
  });

  test("should give rowCount key 1 value", async () => {
    const dbResult = await userRepository.find(client, "serkanerip");
    expect(dbResult).toHaveProperty("rows");
    expect(client.query).toBeCalledTimes(1);
    expect(client.query).toBeCalledWith(
      "SELECT * FROM users WHERE username=$1",
      ["serkanerip"]
    );
  });

  test("should create new user", async () => {
    const dbResult = await userRepository.create(client, {});
    expect(dbResult).toHaveProperty("rows");
    expect(client.query).toBeCalledTimes(1);
  });

  test("should delete a user", async () => {
    const user = { userName: "serkanerip" };
    const dbResult = await userRepository.deleteUser(client, user);
    expect(dbResult).toHaveProperty("rows");
    expect(client.query).toBeCalledTimes(1);
  });

  test("should update a user", async () => {
    const user = { userName: "serkanerip", name: "serkan", password: "123456" };
    const dbResult = await userRepository.update(client, user);
    expect(dbResult).toHaveProperty("rows");
    expect(client.query).toBeCalledTimes(1);
  });
});
```

Testlerimizi yazdık, fonksiyonlarımızın hepsi database ile işlem yapabilmeleri için pg modülünden oluşturduğumuz client
nesnesini ve gereken bazı parametreleri alıyor. Test dosyamızı çalıştıralım.

```bash
./node_modules/.bin/jest __test__/UserRepository.test.js --watch
```

**Sonuçlar**:

```bash
Test Suites: 1 failed, 1 total
Tests:       5 failed, 5 total
Snapshots:   0 total
Time:        0.447s, estimated 1s
Ran all test suites matching /__test__\/UserRepository.test.js/i.
```

Gördüğümüz üzere bütün testlerimiz fail :collision: oldu çıktı mesajı burda gördüğünüzden daha uzun olacak çünkü hataları barındıracak.
Şimdi bu testlerin geçmesi için kodları yazmaya başlıyalım.

**./src/repository/UserRepository.js**:

```js
async function getAll(client) {
  const result = await client.query("SELECT * FROM users");
  return result;
}

async function find(client, username) {
  const result = await client.query("SELECT * FROM users WHERE username=$1", [
    username,
  ]);
  return result;
}

async function create(client, user) {
  const result = await client.query(
    "INSERT INTO users VALUES($1,$2,$3) RETURNING *",
    [user.userName, user.password, user.name]
  );
  return result;
}

async function deleteUser(client, user) {
  const result = await client.query(
    "DELETE FROM users WHERE username=$1 RETURNING username",
    [user.username]
  );
  return result;
}

async function update(client, user) {
  const result = await client.query(
    "UPDATE users SET name=? WHERE username=$2 RETURNING *",
    [user.name, user.password]
  );
  return result;
}

module.exports = {
  getAll,
  find,
  create,
  deleteUser,
  update,
};
```

Tekrardan test dosyamızı çalıştıralıp sonuçları görelim.

**Sonuçlar**:

<pre><span style="background-color:#8AE234"><font color="#3F3F3F"><b> PASS </b></font></span> __test__/<b>UserRepository.test.js</b>
  User Repository Tests
    <font color="#4E9A06">✓</font> should give all users as array (5ms)
    <font color="#4E9A06">✓</font> should give rowCount key 1 value
    <font color="#4E9A06">✓</font> should create new user (1ms)
    <font color="#4E9A06">✓</font> should delete a user (1ms)
    <font color="#4E9A06">✓</font> should update a user

<b>Test Suites: </b><font color="#8AE234"><b>1 passed</b></font>, 1 total
<b>Tests:       </b><font color="#8AE234"><b>5 passed</b></font>, 5 total
<b>Snapshots:   </b>0 total
<b>Time:</b>        0.353s, estimated 1s
<font color="#AAAAAA">Ran all test suites matching /__test__\/UserRepository.test.js/i.</font></pre>

Ve evet testlerimizin hepsi başarıyla geçti. Şimdide gereken son test dosyamızı oluşturup kodluyacağız.
Öncesinde biraz UserService dosyamızdan bahsedelim. Servis katmanında kullanıcının database ile arasındaki bağı oluşturuyoruz.
Bunun için UserRepository'deki fonksiyonlara ihtiyacımız olacak.

Burada'da testimizin sadece UserService birimini test etmesi için iki adet mock işlemi yapmalıyız.
UserRepository fonksiyonunu kullanacaği için servisimiz bu repoyu mocklamamız lazım. Ve bu repo fonksiyonlarıda parametre olarak
pg modülünden oluşturulan client nesnesini aldığı için bunuda mocklamamız gerekiyor.

**UserService.test.js**:

```js
const { Pool } = require("pg");
const userService = require("../src/service/UserService");
const userRepository = require("../src/repository/UserRepository");
const User = require("../src/model/User");

jest.mock("../src/repository/UserRepository", () => {
  const mockUsers = [
    {
      username: "test",
      name: "John Doe",
      password: "toor",
    },
  ];
  return {
    getAll: jest.fn((client) => {
      return { rows: mockUsers, rowCount: 1 };
    }),
    find: jest.fn((client, username) => {
      return { rows: mockUsers.filter((u) => u.username == username) };
    }),
    create: jest.fn((client, user) => {
      return { rows: [user], rowCount: 1 };
    }),
    deleteUser: jest.fn((client, user) => {
      return {
        rows: [{ username: user.username }],
      };
    }),
  };
});

let pool, client;

describe("User Service Tests", () => {
  beforeEach(async () => {
    pool = new Pool();
    client = {};
  });
});
```

Test dosyamızı, testlerimizi yapmak için hazırladık şimdi testlerimizi yazmaya geçelim.

**UserService.test.js**:

```js
const { Pool } = require("pg");
const userService = require("../src/service/UserService");
const userRepository = require("../src/repository/UserRepository");
const User = require("../src/model/User");

jest.mock("../src/repository/UserRepository", () => {
  const mockUsers = [
    {
      username: "test",
      name: "John Doe",
      password: "toor",
    },
  ];
  return {
    getAll: jest.fn((client) => {
      return { rows: mockUsers, rowCount: 1 };
    }),
    find: jest.fn((client, username) => {
      return { rows: mockUsers.filter((u) => u.username == username) };
    }),
    create: jest.fn((client, user) => {
      return { rows: [user], rowCount: 1 };
    }),
    deleteUser: jest.fn((client, user) => {
      return {
        rows: [{ username: user.username }],
      };
    }),
  };
});

let pool, client;

describe("User Service Tests", () => {
  beforeEach(async () => {
    pool = new Pool();
    client = {};
  });

  test("should fetch all users", async () => {
    const users = await userService.getAll(client, userRepository);
    expect(Array.isArray(users)).toBe(true);
    expect(userRepository.getAll).toBeCalled();
  });

  test("should fetch spesific user", async () => {
    const username = "test";
    const user = await userService.findUserByUsername(
      client,
      userRepository,
      username
    );
    expect(userRepository.find).toBeCalled();
    expect(user.username).toBe(username);
  });

  test("should throw not found", async () => {
    const username = "test2";
    const fetchUser = async () =>
      await userService.findUserByUsername(client, userRepository, username);

    await expect(fetchUser()).rejects.toThrow();
    await expect(userRepository.find).toBeCalled();
  });

  test("should create user", async () => {
    const mockUser = {
      username: "test",
      name: "Alley",
      password: "123456",
    };
    const createdUser = await userService.create(
      client,
      userRepository,
      mockUser
    );
    expect(createdUser).toBe(mockUser);
  });

  test("should delete user", async () => {
    const mockUser = User();
    mockUser.userName = "test";
    const deletedUsername = await userService.deleteUserByUsername(
      client,
      userRepository,
      mockUser
    );
    expect(deletedUsername).toBe(mockUser.userName);
  });

  test("should throw an error when trying to delete a user who is not", async () => {
    const mockUser = User();
    mockUser.userName = "notexists";
    const deleteUser = async () =>
      await userService.deleteUserByUsername(client, userRepository, mockUser);
    expect(deleteUser()).rejects.toThrow();
  });
});
```

Test dosyamızın son hali bu şekilde olacaktır şimdi bu testlerimizi çalıştıralım bakalım.

  <pre><b>Test Suites: </b><font color="#EF2929"><b>1 failed</b></font>, 1 total
  <b>Tests:       </b><font color="#EF2929"><b>5 failed</b></font>, <font color="#8AE234"><b>1 passed</b></font>, 6 total
  <b>Snapshots:   </b>0 total
  <b>Time:</b>        0.679s, estimated 1s
  <font color="#AAAAAA">Ran all test suites matching /__test__\/UserService.test.js/i.</font>
  </pre>

Evet testlerimiz fail :collision: oldu beklediğimiz gibi şimdi bu testlerin geçmesi için gereken kodları yazalım.

**src/service/UserService.js**:

```js
async function getAll(client, userRepo) {
  let dbResult;
  try {
    dbResult = await userRepo.getAll(client);
  } catch (e) {}
  return dbResult.rows;
}

async function findUserByUsername(client, userRepo, username) {
  const dbResult = await userRepo.find(client, username);
  if (dbResult.rows.length < 1) {
    throw new Error("User not found!");
  }
  return dbResult.rows[0];
}

async function create(client, userRepo, user) {
  const dbResult = await userRepo.create(client, user);
  return dbResult.rows[0];
}

async function deleteUserByUsername(client, userRepo, user) {
  const findedUser = await findUserByUsername(client, userRepo, user.userName);
  const dbResult = await userRepo.deleteUser(client, findedUser);
  return dbResult.rows[0].username;
}

module.exports = {
  getAll,
  findUserByUsername,
  create,
  deleteUserByUsername,
};
```

UserService metodlarımızı yazdık testlerimizi tekrardan çalıştıralım ve sonuçlara bakalım.

**Sonuçlar**:

<pre><span style="background-color:#8AE234"><font color="#3F3F3F"><b> PASS </b></font></span> __test__/<b>UserService.test.js</b>
  User Service Tests
    <font color="#4E9A06">✓</font> should fetch all users (5ms)
    <font color="#4E9A06">✓</font> should fetch spesific user (8ms)
    <font color="#4E9A06">✓</font> should throw not found (5ms)
    <font color="#4E9A06">✓</font> should create user (1ms)
    <font color="#4E9A06">✓</font> should delete user (1ms)
    <font color="#4E9A06">✓</font> should throw an error when trying to delete a user who is not (1ms)

<b>Test Suites: </b><font color="#8AE234"><b>1 passed</b></font>, 1 total
<b>Tests:       </b><font color="#8AE234"><b>6 passed</b></font>, 6 total
<b>Snapshots:   </b>0 total
<b>Time:</b>        0.577s, estimated 1s
<font color="#AAAAAA">Ran all test suites matching /__test__\/UserService.test.js/i.</font></pre>

Ve görüldüğü üzere testlerimiz başarıyla geçti.

## Açıklamalar

* describe: İlişkili testlerimizi bir arada yazabilmek için describe altında yazıyoruz.
* test: Testimizi yazdığımız alan.
* beforeEach: Bu fonksiyon her testimizden önce yapılması gereken işlemleri yapmamıza yarıyor.

## İmplementasyon

Şimdi gerekli implementasyonları yapıp uygulamamızı bir deniyelim. Öncelikle ihtiyacımız olan şey pg modülünden bir client
nesnesi oluşturmamız gerekiyor testlerimizde mocklayarak fonksiyonlarımıza yollamıştık ancak gerçekten veritabanı işlemleri
yapmak istiyorsak ihtiyacımız olacak.

**src/db.js**:

```js
const { Pool } = require("pg");

const pool = new Pool({
  user: "postgres",
  host: "localhost",
  database: "playaround",
  password: "toor",
  port: 5432,
});

module.exports = {
  pool,
};
```

**index.js**:

```js
const { pool } = require("./src/db");
const UserRepository = require("./src/repository/UserRepository");
const User = require("./src/model/User");
const UserService = require("./src/service/UserService");

const beginTransaction = async (client) => await client.query("BEGIN");
const rollbackTransaction = async (client) => await client.query("ROLLBACK");

void (async function () {
  const client = await pool.connect();
  await beginTransaction(client);

  const user = User();
  user.name = "Serkan";
  user.userName = "serkanerip";
  user.password = "serkan619";
  const createdUser = await UserService.create(client, UserRepository, user);

  console.log(`Oluşturulan Kullanıcı: ${createdUser.username}`);

  users = await UserService.getAll(client, UserRepository);
  console.log("Kullanıcılar:");
  users.forEach((user) => console.log(`${user.username}`));

  await rollbackTransaction(client);
})();
```

Evet yazının sonuna geldik. Burada yazının çok uzamaması için her testin tek tek açıklamasını yapmıyorum
anlayabilecek düzeyde olduğunuz varsayılmıştır. 

Eğer anlamadığınız noktalar olursa benimle iletişime geçmekten çekinmeyin.

Happy Codding :tada: :tada:
