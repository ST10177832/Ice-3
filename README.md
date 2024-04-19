# Ice-3


import java.util.regex.Matcher;
import java.util.regex.Pattern;
import java.util.HashMap;
import java.util.Map;

// Define the User class
class User {
    private String username;
    private String password;
    private String firstName;
    private String lastName;

    // Constructor to create a new user
    public User(String username, String password, String firstName, String lastName) {
        this.username = username;
        this.password = password;
        this.firstName = firstName;
        this.lastName = lastName;
    }

    // Getters for the user's properties
    public String getUsername() {
        return username;
    }

    public String getPassword() {
        return password;
    }

    public String getFirstName() {
        return firstName;
    }

    public String getLastName() {
        return lastName;
    }
}

// Define the Login class
class Login {
    private Map<String, User> users;

    public Login() {
        users = new HashMap<>();
    }

    // Check that the username is valid
    public boolean checkUserName(String username) {
        Pattern pattern = Pattern.compile("^\\w{1,5}_\\w*$");
        Matcher matcher = pattern.matcher(username);
        return matcher.matches();
    }

    // Check that the password is strong
    public boolean checkPasswordComplexity(String password) {
        // Password must be at least 8 characters long and contain at least one uppercase letter, one lowercase letter, one digit, and one special character
        Pattern pattern = Pattern.compile("^(?=.*[A-Z])(?=.*[a-z])(?=.*[0-9])(?=.*[!@#$%^&*()_+-=]).{8,}$");
        Matcher matcher = pattern.matcher(password);
        return matcher.matches();
    }

    // Register a new user
    public String registerUser(String username, String password, String firstName, String lastName) {
        if (!checkUserName(username)) {
            return "Username is not correctly formatted, please ensure that your username contains an underscore and is no more than 5 characters in length.";
        }

        if (!checkPasswordComplexity(password)) {
            return "Password is not correctly formatted, please ensure that the password contains at least 8 characters, a capital letter, a number and a special character.";
        }

        if (users.containsKey(username)) {
            return "Username already exists.";
        }

        User user = new User(username, password, firstName, lastName);
        users.put(username, user);

        return "User registered successfully.";
    }

    // Log in with a username and password
    public String loginUser(String username, String password) {
        if (users.containsKey(username)) {
            User user = users.get(username);
            if (user.getPassword().equals(password)) {
                return "Welcome " + user.getFirstName() + " " + user.getLastName() + ", it is great to see you again.";
            } else {
                return "Username or password incorrect, please try again.";
            }
        } else {
            return "Username or password incorrect, please try again.";
        }
    }
}

// Main class for running the program
public class Main {
    public static void main(String[] args) {
        Login login = new Login();

        // Register a new user
        String registrationMessage = login.registerUser("user_1", "Password1!", "Cynthia", "Shabalala");
        System.out.println(registrationMessage);

        // Try to register a user with the same username
        registrationMessage = login.registerUser("user_1", "Password2@", "Sam", "Ndhlovu");
        System.out.println(registrationMessage);

        // Log in with a valid username and password
        String loginMessage = login.loginUser("user_1", "Password1!");
        System.out.println(loginMessage);

        // Log in with an invalid username and password
        loginMessage = login.loginUser("user_2", "Password1!");
        System.out.println(loginMessage);
    }
}
