
> [!WARNING]
> When using <xref:System.Text.RegularExpressions> to process untrusted input, pass a timeout. A malicious user can provide input to `RegularExpressions`, causing a [Denial-of-Service attack](https://www.us-cert.gov/ncas/tips/ST04-015). ASP.NET Core framework APIs that use `RegularExpressions` pass a timeout.
