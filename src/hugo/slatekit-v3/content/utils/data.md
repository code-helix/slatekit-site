
# Data

<table class="table table-striped table-bordered">
  <tbody>
    <tr>
      <td><strong>desc</strong></td>
      <td>Database access utilty to query and manage data using JDBC for MySql. Other database support coming later.</td>
    </tr>
    <tr>
      <td><strong>date</strong></td>
      <td>{{% sk-version-date %}}</td>
    </tr>
    <tr>
      <td><strong>version</strong></td>
      <td>{{% sk-version-number %}}</td>
    </tr>
    <tr>
      <td><strong>jar</strong></td>
      <td>slatekit.common.jar</td>
    </tr>
    <tr>
      <td><strong>namespace</strong></td>
      <td>slatekit.common.db</td>
    </tr>
    <tr>
      <td><strong>artifact</strong></td>
      <td>com.slatekit:slatekit-common</td>
    </tr>
    <tr>
      <td><strong>source folder</strong></td>
      <td><a href="https://github.com/slatekit/slatekit/tree/master/src/lib/kotlin/slatekit-common/src/main/kotlin/slatekit/common/db" class="url-ch">src/lib/kotlin/slatekit-common/src/main/kotlin/slatekit/common/db</a></td>
    </tr>
    <tr>
      <td><strong>example</strong></td>
      <td><a href="https://github.com/slatekit/slatekit/tree/master/src/lib/kotlin/slatekit-examples/src/main/kotlin/slatekit/examples/Example_Database.kt" class="url-ch">src/lib/kotlin/slate-examples/src/main/kotlin/slatekit/examples/Example_Database.kt</a></td>
    </tr>
    <tr>
      <td><strong>depends on</strong></td>
      <td> slatekit-results</td>
    </tr>
  </tbody>
</table>
{{% break %}}

## Gradle
{{< highlight gradle >}}
    // other setup ...
    repositories {
        maven { url  "https://dl.bintray.com/codehelixinc/slatekit" }
    }

    dependencies {
        // other libraries

        // slatekit-common: Utilities for Android or Server
        compile 'com.slatekit:slatekit-common:0.9.17'
    }

{{< /highlight >}}
{{% break %}}

## Import
{{< highlight kotlin >}}


// required 
import slatekit.db.Db
import slatekit.common.db.DbConString


// optional 
import slatekit.results.Try
import slatekit.results.Success
import slatekit.core.cmds.Cmd
import slatekit.entities.core.EntityMapper
import slatekit.examples.common.User
import slatekit.meta.models.ModelMapper
import slatekit.orm.core.OrmMapper
import slatekit.orm.databases.vendors.MySqlConverter




{{< /highlight >}}
{{% break %}}

## Setup
{{< highlight kotlin >}}


n/a


{{< /highlight >}}
{{% break %}}

## Usage
{{< highlight kotlin >}}


        // NOTES:
        // 1. The Db.kt simply uses JDBC
        // 2. There is a separate DbLookup.kt component that
        //    loads, stores, and manages named database connections.
        //    Refer to that example for more info.

        // CASE 1: Create DB connection.
        val con = DbConString(
            "com.mysql.jdbc.Driver",
            "jdbc:mysql://localhost/default",
            "root",
            "abcdefghi"
        )

        // CASE 2. Initialize the DB with the connection string.
        // NOTE: This defaults the db to mysql. The first line is same
        // as db = Db(con, source: DbSourceMySql())
        // In the future, we can more easily support mutliple databases
        // using this approach.
        val db = Db(con)

        // CASE 3: Open the database
        db.open()

        // CASE 4: Get scalar values
        val total1 = db.getScalarString       ("select test_string from db_tests where id = 1", null)
        val total2 = db.getScalarBool         ("select test_bool   from db_tests where id = 1", null)
        val total3 = db.getScalarShort        ("select test_short  from db_tests where id = 1", null)
        val total4 = db.getScalarInt          ("select test_int    from db_tests where id = 1", null)
        val total5 = db.getScalarLong         ("select test_long   from db_tests where id = 1", null)
        val total6 = db.getScalarDouble       ("select test_double from db_tests where id = 1", null)
        val total7 = db.getScalarLocalDate    ("select test_ldate  from db_tests where id = 1", null)
        val total8 = db.getScalarLocalTime    ("select test_ltime  from db_tests where id = 1", null)
        val total9 = db.getScalarLocalDateTime("select test_ldtime from db_tests where id = 1", null)

        // CASE 5: Execute a sql insert
        val id1 = db.insert("insert into `city`(`name`) values( 'ny' )")

        // CASE 6: Execute a sql insert using parameters
        val id2 = db.insert("insert into `city`(`name`) values( ? )", listOf("ny"))

        // CASE 7: Execute a sql update
        val count7 = db.update("update `city` set `alias` = 'nyc' where id = 2")

        // CASE 8: Execute a sql udpate using parameters
        val count8 = db.update("update `city` set `alias` = 'nyc' where id = ?", listOf(id2))

        // CASE 9: Deletes are same as updates
        val count9a = db.update("delete from `city` where id = 2")
        val count9b = db.update("delete from `city` where id = ?", listOf(2))


        // ===============================================================
        // STORED PROCS
        // ===============================================================
        // CASE 10: Call a stored proc that updates data
        val count10 = db.callUpdate("dbtests_activate_by_id", listOf(id2))

        // CASE 11: Call a stored proc that fetches data
        val count11 = db.callQuery("dbtests_max_by_id",
                callback = { rs -> rs.getString(0) }, inputs = listOf(id2))

        // ===============================================================
        // MODELS / MAPPERS
        // ===============================================================
        // CASE 12: Map a record to an model using the mapper component
        // The mapper will load a schema from the User class by checking
        // for "Field" annotations
        val userModelSchema = ModelMapper.loadSchema(User::class)
        val mapper = OrmMapper<Long,User>(userModelSchema, db, Long::class, MySqlConverter())
        val item1 = db.mapOne<User>("select * from `user` where id = 1", mapper)
        println(item1)

        // CASE 13: Map multiple records
        val items = db.mapMany<User>("select * from `user` where id < 5", mapper)
        println(items)

        // CASE 14: Create the table using the model
        // Be careful with this, ensure you are using a connection string
        // with limited permissions
        //createTable(db, userModelSchema)

        // CASE 15: Drop a table
        // Be careful with this, ensure you are using a connection string
        // with limited permissions.
        

{{< /highlight >}}
{{% break %}}

