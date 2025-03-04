To send a `POST` request with a **mutation** in a plain Java application using **SmallRye GraphQL Client**, follow these steps.

---

### **1. Modify the GraphQL Client Interface**
Add a mutation method to the interface:

```java
import io.smallrye.graphql.client.typesafe.api.GraphQLClientApi;
import org.eclipse.microprofile.graphql.Mutation;
import org.eclipse.microprofile.graphql.Query;

import java.util.List;

@GraphQLClientApi
public interface MyGraphQLClient {

    @Query("allUsers")
    List<User> getAllUsers();

    @Query("userById")
    User getUserById(String id);

    @Mutation("createUser")
    User createUser(UserInput input);

    static MyGraphQLClient create(String url) {
        return io.smallrye.graphql.client.GraphQLClientBuilder.newBuilder()
                .endpoint(url)
                .build(MyGraphQLClient.class);
    }
}
```
- **`@Mutation("createUser")`**: Maps to the GraphQL mutation.
- **`UserInput`**: A separate class for the mutation input.

---

### **2. Create a UserInput Class**
GraphQL often expects an input type for mutations:

```java
public class UserInput {
    private String name;
    private String email;

    public UserInput() {}

    public UserInput(String name, String email) {
        this.name = name;
        this.email = email;
    }

    // Getters and Setters
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}
```

---

### **3. Call the Mutation in the Main Method**
Modify `Main.java` to call the mutation:

```java
public class Main {
    public static void main(String[] args) {
        String graphqlEndpoint = "http://localhost:8080/graphql";
        
        MyGraphQLClient client = MyGraphQLClient.create(graphqlEndpoint);

        // Create a new user
        UserInput newUser = new UserInput("John Doe", "john.doe@example.com");
        User createdUser = client.createUser(newUser);
        
        System.out.println("Created User: " + createdUser.getName() + " - " + createdUser.getEmail());
    }
}
```

---

### **4. Expected GraphQL Mutation**
Your GraphQL server should define a mutation similar to this:

```graphql
type Mutation {
    createUser(input: UserInput): User
}

input UserInput {
    name: String!
    email: String!
}
```

---

### **5. Run the Application**
Compile and run:

```sh
mvn clean compile exec:java -Dexec.mainClass="Main"
```

---

### **Summary**
✅ Uses `@Mutation("createUser")` to send a `POST` request.  
✅ Passes `UserInput` as a structured parameter.  
✅ Works in a standalone Java SE application.  

This approach allows **SmallRye GraphQL Client** to send GraphQL mutations efficiently. 🚀 Let me know if you need more details!
