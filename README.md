# NET GlobalizationInvariantMode-String-Compare-Bug
Assertion fails for scenario "if s1 &lt; s2 and s2 &lt; s3, it should follow that s1 &lt; s3"
```
    internal class Program
    {
        static void Main(string[] args)
        {
            var s1 = "бал";
            var s2 = "Бан";
            var s3 = "Д";

            var ci = System.Globalization.CultureInfo.InvariantCulture.CompareInfo;

            // First we pass the 3 strings in upper case with no problems.
            Debug.Assert(ci.Compare(s1.ToUpper(), s2.ToUpper(), System.Globalization.CompareOptions.IgnoreCase) < 0);
            Debug.Assert(ci.Compare(s2.ToUpper(), s3.ToUpper(), System.Globalization.CompareOptions.IgnoreCase) < 0);
            Debug.Assert(ci.Compare(s1.ToUpper(), s3.ToUpper(), System.Globalization.CompareOptions.IgnoreCase) < 0);

            // Now we pass the strings without ToUpper() - theoretically yielding the same result.
            Debug.Assert(ci.Compare(s1, s2, System.Globalization.CompareOptions.IgnoreCase) < 0);
            Debug.Assert(ci.Compare(s2, s3, System.Globalization.CompareOptions.IgnoreCase) < 0);

            // if s1 < s2 and s2 < s3, it should follow that s1 < s3.
            // This assertion fails if the project is running in invariant-globalization mode.
            // It succeecds if not running in invariant-globalization mode.
            //
            // To run in invariant-globalization mode, add the following to the property group
            // in StringCompareBug.csproj:
            //              <InvariantGlobalization>true</InvariantGlobalization>
            Debug.Assert(ci.Compare(s1, s3, System.Globalization.CompareOptions.IgnoreCase) < 0);
        }
    }
```
