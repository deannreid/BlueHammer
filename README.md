# BlueHammer


-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA512

Repository hosting the bluehammer vulnerability

I'm just really wondering what was the math behind their decision, like you knew this was going to happen and you still did whatever you did ? Are they serious ?

-----BEGIN PGP SIGNATURE-----

iHUEARYKAB0WIQRJTvAf/AWVhAKEeb7FFoRCS0/SbAUCac8VlgAKCRDFFoRCS0/S
bK8pAP9CzNnH26FVVdHZWVyDvOIwuZ1np1dTv7T5YaVCjf4tiwD+MC4Ikq+/ywdD
I7dabkH7iSZflULM+hGUOur0mnAg9Qw=
=Enhh
-----END PGP SIGNATURE-----

Edit : There are few bugs in the PoC that could prevent it from working, might fix them later.


### Fixed Bugs
- `SamiChangePasswordUser` sets `UF_PASSWORD_EXPIRED` on admin accounts when called via the raw NTLM hash path. The subsequent `LogonUserEx` call was failing with `ERROR_PASSWORD_MUST_CHANGE` (1907) for any admin user, while low-privilege users were unaffected. Fixed by calling `NetUserGetInfo`/`NetUserSetInfo` (level 1008) immediately after the password swap to clear `UF_PASSWORD_EXPIRED` before logon is attempted.
- `GetTokenInformation` was called unconditionally after `LogonUserEx`, dereferencing a NULL token handle when the logon failed. Added `htoken != NULL` guard to prevent the crash on the admin path.
- Added `#include <lm.h>` and `#pragma comment(lib, "Netapi32.lib")` to support the flag-clearing calls.