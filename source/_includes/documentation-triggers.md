A trigger is an action that is performed when text matching some regular expression is received in a terminal session.
#### What can Triggers Do?
Various actions may be assigned to triggers. These include:
<ul>
        <li>Bounce Dock Icon: Makes the dock icon bounce until the iTerm2 window becomes key.</li>
        <li>Ring Bell: Plays the standard system bell sound once.</li>
        <li>Run Command: Runs a user-defined command.</li>
        <li>Run Coprocess: Runs a <a href="coprocesses.html">Coprocess</a>.</li>
        <li>Send Growl Alert: If Growl is enabled, a Growl alert is sent.</li>
        <li>Send Text: Sends user-defined text back to the terminal as though the user had typed it.</li>
        <li>Show Alert: Shows an alert box with user-defined text.</li>
</ul>
#### What is a Parameter?
Various actions (Run Command, Run Coprocess, Send Growl Alert, Send Text, and Show Alert) require additional information. This is specified in the "Parameters" field. Some special values are allowed here: 
<table>
        <thead>
                <tr>
                        <th>Value</th>
                        <th>Meaning</th>
                </tr>
        </thead>
        <tbody>
                <tr>
                        <td>\0</td>
                        <td>The entire value matched by the regular expression.</td>
                </tr>
                <tr>
                        <td>\1, \2, ..., \9</td>
                        <td>The nth value captured by the regular expression.</td>
                </tr>
                <tr>
                        <td>\a</td>
                        <td>A BEL character (^G).</td>
                </tr>
                <tr>
                        <td>\b</td>
                        <td>A backspace character ^H.</td>
                </tr>
                <tr>
                        <td>\e</td>
                        <td>An ESC character (ascii 27).</td>
                </tr>
                <tr>
                        <td>\n</td>
                        <td>A newline character.</td>
                </tr>
                <tr>
                        <td>\r</td>
                        <td>A linefeed character.</td>
                </tr>
                <tr>
                        <td>\t</td>
                        <td>A tab character.</td>
                </tr>
                <tr>
                        <td>\xNN</td>
                        <td>A hex value NN (for example: \x1b sends ascii code 27, an ESC).</td>
                </tr>
        </tbody>
</table>
#### Example
The <a href="https://github.com/mmastrac/iterm2-zmodem">iTerm2-zmodem</a> project demonstrates hooking up iTerm2 to zmodem upload and download.
#### Technical Details
Regular expressions conform to the <a href="http://userguide.icu-project.org/strings/regexp">ICU regular expressions</a> rules. Text that is written to the screen and bells are sent to the regex matcher for evaluation. Only one line at a time is matched. Matching is performed when a newline or cursor-moving escape code is processed. Lines longer than 1024 characters are truncated at the 1024th character for performance reasons.
#### New features in 2.9 and later
##### Open Password Manager
This trigger opens the password manager. Its parameter is the name of an account in the password manager to select by default.
##### Instant Triggers
Since their inception, triggers would only fire at the end of a line (or in a few other cases, such as when part of the line is erased). The nightly build now has an "Instant" property for each trigger. When <i>Instant</i> is set, the trigger will fire once per line as soon as the match occurs, without waiting for a newline. This was added for the benefit of the <i>Open Password Manager</i> trigger, since password prompts usually are not followed by a newline. This may cause certain regular expressions (for example, ".*") to match less than they otherwise might.
