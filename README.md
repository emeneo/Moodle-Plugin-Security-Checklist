https://docs.moodle.org/dev/Plugin_contribution_checklist#Security

Never trust the user input.
Do not access superglobals like $_REQUEST directly, use wrappers like required_param() with correct type declared to sanitize input.
Always use placeholders in custom SQL queries (? or :named).
Always check for the sesskey before taking an action on submitted data.
Check for require_login().
Always check that the user has appropriate capabilities before displaying the widgets and before taking the actual action.
Avoid using malicious functions like call_user_func(), eval(), unserialize() and so on, especially when they would be called with user-supplied data


From https://moodle.org/mod/forum/discuss.php?d=263614#p1142448 :

Prevent the cross-site request forgery

I was quite surprised how often this happens. Using the sesskey() feature to prevent CSRF attacks is a basic security protection you should include in your code whenever you are performing an action. When using the forms API, session key is checked implicitly. In all other cases it's your duty to perform the check. See this page for more details.
Check capabilities

It is important to highlight that permission checks must be done at both places. Not only there where your code is deciding whether or not a certain action link should be displayed (has_capability() is typically used there). It is essential to also check it just before you actually perform the action (that's where you want to call require_capability()). Remember, your code is open source. If require_capability() is missing, users can easily bypass your "protection" by submitting the data directly.
Sanitize user's input

You should never need to access superglobals $_GET, $_POST and $_COOKIE directly. Moodle provides wrappers for getting HTTP parameters - such as optional_param() and required_param(). Make sure you pick the most specific PARAM type for each of the parameters.




