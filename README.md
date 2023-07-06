# ThrowIf
A set of basic, fluent validation extensions.  Each is set up to to throw an appropriate error (or a custom one) on fail condition, or simply return the passed value on success, allowing for fluent syntax.

- Works with value, reference, nullable variables
- Can be chained (fluent) for multiple tests or "on the fly" testing.

Basic usage:

Generally each extension takes the following:

- The argument to test.
- optional parameters to test against (min and max values for range, for instance).
- optional name of the variable to be used when throwing ArgumentException.
- optional exception to throw if test fails, in case ArgumentException won't cut it.



Examples

```c#
// sets a variable if within range, otherwise throws an ArgumentOutOfRangeException.  Works for any IComparable
int test = 11;
var result = test.ThrowIfOutOfRange(1, 10);

var test = 'd';
var result = test.ThrowIfOutOfRange('a', 'c');


// throws if null or default.  Works for reference or value types, nullables as well
string test = default!;
string result = test.ThrowIfNullOrDefault(new Exception("Invalid!"));

string? test = default;
string? result = test.ThrowIfNullOrDefault(new Exception("Invalid!"));             
                  
// custom testing via func
var testString = "The quick brown fox jumped over the lazy dog.";
var resultChar = testString.ThrowIf(x => x.Contains("dog"), nameof(testString));

var testString = "The quick brown fox jumped over the lazy dog.";
var resultChar = testString.ThrowIfNot(x => x.Contains("cat"), nameof(testString));

// fluent use
string testString = "I am a lazy dog.";
string illegalChars = @" ./\{}";

var validPassword = testString
    .ThrowIfNullOrWhitespace(nameof(testString))
    .ThrowIf(x => x.IndexOfAny(illegalChars.ToCharArray()) > 0, new ArgumentException($"Contains illegal characters: {illegalChars}", nameof(testString)))
    .ThrowIfNot(x => x.Length > 5, new ArgumentException("Is less than 5 characters.", nameof(testString)))
    .ThrowIfNot(x => x.Any(y => char.IsUpper(y)), new ArgumentException("Does not contain an upper-case letter."))
    .ThrowIfNot(x => x.Any(y => char.IsLower(y)), new ArgumentException("Does not contain a lower-case letter."))
    .ThrowIfNot(x => x.Any(y => char.IsNumber(y)), new ArgumentException("Does not contain a number."));

```

