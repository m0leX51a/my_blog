# Sql Injection source code review

### Vulnerable  code

 

```csharp
using System.Data.SqlClient;

public class VulnerableCode
{
    public void Login(string username, string password)
    {
        string connectionString = "Server=myServerAddress;Database=myDataBase;User Id=myUsername;Password=myPassword;";
        string query = "SELECT * FROM users WHERE username = '" + username + "' AND password = '" + password + "'";

        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();
            SqlCommand command = new SqlCommand(query, connection);
            SqlDataReader reader = command.ExecuteReader();

            if (reader.Read())
            {
                Console.WriteLine("Login successful!");
            }
            else
            {
                Console.WriteLine("Invalid username or password.");
            }
        }
    }
}
```

### mitigated code

```csharp
using System.Data.SqlClient;

public class SecureCode
{
    public void Login(string username, string password)
    {
        string connectionString = "Server=myServerAddress;Database=myDataBase;User Id=myUsername;Password=myPassword;";
        string query = "SELECT * FROM users WHERE username = @username AND password = @password";

        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();
            SqlCommand command = new SqlCommand(query, connection);
            command.Parameters.AddWithValue("@username", username);
            command.Parameters.AddWithValue("@password", password);
            SqlDataReader reader = command.ExecuteReader();

            if (reader.Read())
            {
                Console.WriteLine("Login successful!");
            }
            else
            {
                Console.WriteLine("Invalid username or password.");
            }
        }
    }
}
```