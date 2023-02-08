# Code samples

## Write Minidump file programmatically

`MiniDumpWriteDump` function [description](https://docs.microsoft.com/en-us/windows/win32/api/minidumpapiset/nf-minidumpapiset-minidumpwritedump).  
`MINIDUMP_TYPE` enumeration [description](https://docs.microsoft.com/en-us/windows/win32/api/minidumpapiset/ne-minidumpapiset-minidump_type).

``` csharp
public enum MinidumpType : uint
		{
			MiniDumpNormal = 0x00000000,
			MiniDumpWithDataSegs = 0x00000001,
			MiniDumpWithFullMemory = 0x00000002,
			MiniDumpWithHandleData = 0x00000004,
			MiniDumpFilterMemory = 0x00000008,
			MiniDumpScanMemory = 0x00000010,
			MiniDumpWithUnloadedModules = 0x00000020,
			MiniDumpWithIndirectlyReferencedMemory = 0x00000040,
			MiniDumpFilterModulePaths = 0x00000080,
			MiniDumpWithProcessThreadData = 0x00000100,
			MiniDumpWithPrivateReadWriteMemory = 0x00000200,
			MiniDumpWithoutOptionalData = 0x00000400,
			MiniDumpWithFullMemoryInfo = 0x00000800,
			MiniDumpWithThreadInfo = 0x00001000,
			MiniDumpWithCodeSegs = 0x00002000,
			MiniDumpWithoutAuxiliaryState = 0x00004000,
			MiniDumpWithFullAuxiliaryState = 0x00008000,
			MiniDumpWithPrivateWriteCopyMemory = 0x00010000,
			MiniDumpIgnoreInaccessibleMemory = 0x00020000,
			MiniDumpWithTokenInformation = 0x00040000,
			MiniDumpWithModuleHeaders = 0x00080000,
			MiniDumpFilterTriage = 0x00100000,
			MiniDumpWithAvxXStateContext = 0x00200000,
			MiniDumpWithIptTrace = 0x00400000,
			MiniDumpScanInaccessiblePartialPages = 0x00800000,
			MiniDumpFilterWriteCombinedMemory,
			MiniDumpValidTypeFlags = 0x01ffffff
		};

		//typedef struct _MINIDUMP_EXCEPTION_INFORMATION {
		//    DWORD ThreadId;
		//    PEXCEPTION_POINTERS ExceptionPointers;
		//    BOOL ClientPointers;
		//} MINIDUMP_EXCEPTION_INFORMATION, *PMINIDUMP_EXCEPTION_INFORMATION;
		[StructLayout(LayoutKind.Sequential, Pack = 4)]  // Pack=4 is important! So it works also for x64!
		struct MiniDumpExceptionInformation
		{
			public uint ThreadId;
			public IntPtr ExceptionPointers;
			[MarshalAs(UnmanagedType.Bool)]
			public bool ClientPointers;
		}

		[DllImport("dbghelp.dll", EntryPoint = "MiniDumpWriteDump", CallingConvention = CallingConvention.StdCall, CharSet = CharSet.Unicode, ExactSpelling = true, SetLastError = true)]
		static extern bool MiniDumpWriteDump(IntPtr hProcess, uint processId, SafeHandle hFile, uint dumpType, ref MiniDumpExceptionInformation expParam, IntPtr userStreamParam, IntPtr callbackParam);

		// Overload supporting MiniDumpExceptionInformation == NULL
		[DllImport("dbghelp.dll", EntryPoint = "MiniDumpWriteDump", CallingConvention = CallingConvention.StdCall, CharSet = CharSet.Unicode, ExactSpelling = true, SetLastError = true)]
		static extern bool MiniDumpWriteDump(IntPtr hProcess, uint processId, SafeHandle hFile, uint dumpType, IntPtr expParam, IntPtr userStreamParam, IntPtr callbackParam);

		[DllImport("kernel32.dll", EntryPoint = "GetCurrentThreadId", ExactSpelling = true)]
		static extern uint GetCurrentThreadId();
		
		public static bool Write(string fileName, MinidumpType dumpType)
		{
			using (var fs = new FileStream(fileName, FileMode.Create, FileAccess.Write, FileShare.None))
			{
				Process currentProcess = Process.GetCurrentProcess();
				IntPtr currentProcessHandle = currentProcess.Handle;
				uint currentProcessId = (uint)currentProcess.Id;
				MiniDumpExceptionInformation exp;
				exp.ThreadId = GetCurrentThreadId();
				exp.ClientPointers = false;
				exp.ExceptionPointers = Marshal.GetExceptionPointers();
				bool bRet = false;
				if (exp.ExceptionPointers == IntPtr.Zero)
					bRet = MiniDumpWriteDump(currentProcessHandle, currentProcessId, fs.SafeFileHandle, (uint)dumpType, IntPtr.Zero, IntPtr.Zero, IntPtr.Zero);
				else
					bRet = MiniDumpWriteDump(currentProcessHandle, currentProcessId, fs.SafeFileHandle, (uint)dumpType, ref exp, IntPtr.Zero, IntPtr.Zero);
				return bRet;
			}
		}
```

## Functional way for null check
``` csharp
var test = DataContext?.Message;
```
can be replaced by

``` csharp
var text = DataContext.IfDefined(m => m.Message);
```
``` csharp
public static TResult IfDefined<T, TResult>(this T x, Func<T, TResult> f, Func<TResult> otherwise = null)
{
    if (x != null)
    {
        return f(x);
    }
    if (otherwise != null)
    {
        return otherwise();
    }
    return default(TResult);
}
```

## IHttpClientFactory with DI

Following nuget packages are required
> Microsoft.Extensions.DependencyInjection  
> Microsoft.Extensions.Http  
> System.Net.Http  

First we have to create new instance of `ServiceCollection` and register services.

``` csharp
serviceProvider = new ServiceCollection()
	.AddHttpClient()
	.AddTransient<IRestApiClient, RestApiClient>()
	.BuildServiceProvider();
```

`IRestApiClient` interface implementation:
``` csharp
public interface IRestApiClient
{
	Task<string> PostMessageAsync(string data);
}
```

`RestApiClient` class implementation

``` csharp
public class RestApiClient : IRestApiClient
{
    private IHttpClientFactory _httpClientFactory;    
    private ILogger<RestApiClient> _logger;

    public RestApiClient(IHttpClientFactory httpClientFactory, ILogger<RestApiClient> logger)
    {
        _logger = logger;
        _httpClientFactory = httpClientFactory;
    }
    
    public async Task<string> PostMessageAsync(string data)
    {
		// Get HttpClient instance.
        var client = _httpClientFactory.CreateClient();
		...
    }
}
```

Then finally if we want to create instance of `RestApiClient` class, we call `GetService`. Behind the scene the `Activator` creates instance of `RestApiClass`.

``` csharp
var apiClient = serviceProvider.GetService<IRestApiClient>();
```

## .NET 6 Minimal API Web server

```csharp
var app = WebApplication.Create();
app.MapGet("/", () => "Hello World!");
app.MapGet("api/variable", () => new
{
    Variable = "test value"
});

await app.RunAsync();

```