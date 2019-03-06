# Port Document
This document is being used to track how this project will be ported to an ARM implementation.  The `PORTING` document in `IBM-TPM-1.2/tpm` will be directly referenced here.

## Porting - Interface
First Decision
- Doesn't have to be ported since the port will not include a client server model, it will only be accessible from the same system
- This includes the files `tpm_io.c / h` and `tpm_server.c / h`

## Porting - Platform Dependent Code
1) Crypto
- `tpm_crypto.c`: Crypto API dependent -> Library dependent
  - Currently supports OpenSSL and PCIXCC API
  - The board DOES NOT support accellerated crypto
    - Have to look if OpenSSL can be cross-compiled to ARM
    - wolfSSL looks like a very promising alternative
- `tpm_crypoth.c`: Higher level functions -> Platform dependent
  
2) Trace
- `debug.h`: Uses `printf` for debugging
- Probably won't need to change

3) NVRAM
- `tpm_nvfile.c`: Abstraction that holds non-volatile data
  - Dependencies: `tpm_nvram_const.h` and `tpm_nvfile.h`
- Needs to be converted into specific NVRAM implementation

4) Time
- `tpm_time.c`: Time used for authentication dictionary mitigation and tick stamps
- **Requires 64-bit integers for some time structures**
  - May be annoying to deal with since development is occuring on a 32-bit architecture

5) Threads
- Development is on single core device so this will most likely not be ported