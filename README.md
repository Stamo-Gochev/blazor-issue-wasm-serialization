Link to the public issue:

https://github.com/dotnet/aspnetcore/issues/52083

### Describe the bug

An error is thrown when making a JS interop call in a **published** blazor wasm app that should serialize a `KeyValuePair` [value](https://github.com/Stamo-Gochev/blazor-issue-wasm-serialization/blob/master/BlazorissueWasmSerialization/BlazorissueWasmSerialization.Client/Pages/Home.razor#L29).

This seems to be related to trimming as running this with things like `dotnet run --configuration Release` works as expected, but publishing the app starts the trimmer by default.

In addition, using an [anonymous type instead](https://github.com/Stamo-Gochev/blazor-issue-wasm-serialization/blob/master/BlazorissueWasmSerialization/BlazorissueWasmSerialization.Client/Pages/Home.razor#L35-L48) of `KeyValuePair` works without any errors.

> [!NOTE]
> I tried to [disable the trimming](https://learn.microsoft.com/en-us/dotnet/core/deploying/trimming/trimming-options?pivots=dotnet-7-0#enable-trimming), but the error is still present. Any suggestions on how to do that will be helpful - this might turn out to be another issue though.

![blazor-issue-wasm-serialization](https://github.com/dotnet/aspnetcore/assets/1857705/b96574f5-7e82-44bc-b408-9f39b0c5e810)

### Expected Behavior

No error is thrown.

### Steps To Reproduce

1. Clone https://github.com/Stamo-Gochev/blazor-issue-wasm-serialization
2. Change directory to https://github.com/Stamo-Gochev/blazor-issue-wasm-serialization/tree/master/BlazorissueWasmSerialization/BlazorissueWasmSerialization
3. Run `dotnet publish --configuration Release`
4. Go to `BlazorissueWasmSerialization/BlazorissueWasmSerialization/bin/Release/net8.0/publish`
5. Run `dotnet BlazorissueWasmSerialization.dll`
6. [Click the "Show issue button"](https://github.com/Stamo-Gochev/blazor-issue-wasm-serialization/blob/master/BlazorissueWasmSerialization/BlazorissueWasmSerialization.Client/Pages/Home.razor#L19-L33)

### Exceptions (if any)

<details>

<summary>Exception</summary>

```bash
blazor.web.js:1 crit: Microsoft.AspNetCore.Components.WebAssembly.Rendering.WebAssemblyRenderer[100]
      Unhandled exception rendering component: ConstructorContainsNullParameterNames, System.Collections.Generic.KeyValuePair`2[System.String,System.String] SerializationNotSupportedParentType, System.Object Path: $.
System.NotSupportedException: ConstructorContainsNullParameterNames, System.Collections.Generic.KeyValuePair`2[System.String,System.String] SerializationNotSupportedParentType, System.Object Path: $.
 ---> System.NotSupportedException: ConstructorContainsNullParameterNames, System.Collections.Generic.KeyValuePair`2[System.String,System.String]
   at System.Text.Json.ThrowHelper.ThrowNotSupportedException_ConstructorContainsNullParameterNames(Type )
   at System.Text.Json.Serialization.Metadata.DefaultJsonTypeInfoResolver.PopulateParameterInfoValues(JsonTypeInfo )
   at System.Text.Json.Serialization.Metadata.DefaultJsonTypeInfoResolver.CreateTypeInfoCore(Type , JsonConverter , JsonSerializerOptions )
   at System.Text.Json.Serialization.Metadata.DefaultJsonTypeInfoResolver.CreateJsonTypeInfo(Type , JsonSerializerOptions )
   at System.Text.Json.Serialization.Metadata.DefaultJsonTypeInfoResolver.GetTypeInfo(Type , JsonSerializerOptions )
   at System.Text.Json.JsonSerializerOptions.GetTypeInfoNoCaching(Type )
   at System.Text.Json.JsonSerializerOptions.CachingContext.CreateCacheEntry(Type type, CachingContext context)
--- End of stack trace from previous location ---
   at System.Text.Json.JsonSerializerOptions.CachingContext.CacheEntry.GetResult()
   at System.Text.Json.JsonSerializerOptions.CachingContext.GetOrAddTypeInfo(Type , Boolean )
   at System.Text.Json.JsonSerializerOptions.GetTypeInfoInternal(Type , Boolean , Nullable`1 , Boolean , Boolean )
   at System.Text.Json.Serialization.Metadata.JsonTypeInfo.Configure()
   at System.Text.Json.Serialization.Metadata.JsonTypeInfo.<EnsureConfigured>g__ConfigureSynchronized|172_0()
   at System.Text.Json.Serialization.Metadata.JsonTypeInfo.EnsureConfigured()
   at System.Text.Json.JsonSerializerOptions.GetTypeInfoInternal(Type , Boolean , Nullable`1 , Boolean , Boolean )
   at System.Text.Json.WriteStackFrame.InitializePolymorphicReEntry(Type , JsonSerializerOptions )
   at System.Text.Json.Serialization.JsonConverter.ResolvePolymorphicConverter(Object , JsonTypeInfo , JsonSerializerOptions , WriteStack& )
   at System.Text.Json.Serialization.JsonConverter`1[[System.Object, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TryWrite(Utf8JsonWriter , Object& , JsonSerializerOptions , WriteStack& )
   at System.Text.Json.Serialization.Converters.DictionaryOfTKeyTValueConverter`3[[System.Collections.Generic.Dictionary`2[[System.String, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.Object, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.String, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.Object, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].OnWriteResume(Utf8JsonWriter , Dictionary`2 , JsonSerializerOptions , WriteStack& )
   at System.Text.Json.Serialization.JsonDictionaryConverter`3[[System.Collections.Generic.Dictionary`2[[System.String, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.Object, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.String, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.Object, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].OnTryWrite(Utf8JsonWriter , Dictionary`2 , JsonSerializerOptions , WriteStack& )
   at System.Text.Json.Serialization.JsonConverter`1[[System.Collections.Generic.Dictionary`2[[System.String, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.Object, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TryWrite(Utf8JsonWriter , Dictionary`2& , JsonSerializerOptions , WriteStack& )
   at System.Text.Json.Serialization.JsonConverter`1[[System.Collections.Generic.Dictionary`2[[System.String, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.Object, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]], System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TryWriteAsObject(Utf8JsonWriter , Object , JsonSerializerOptions , WriteStack& )
   at System.Text.Json.Serialization.JsonConverter`1[[System.Object, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TryWrite(Utf8JsonWriter , Object& , JsonSerializerOptions , WriteStack& )
   at System.Text.Json.Serialization.Converters.ArrayConverter`2[[System.Object[], System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.Object, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].OnWriteResume(Utf8JsonWriter , Object[] , JsonSerializerOptions , WriteStack& )
   at System.Text.Json.Serialization.JsonCollectionConverter`2[[System.Object[], System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e],[System.Object, System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].OnTryWrite(Utf8JsonWriter , Object[] , JsonSerializerOptions , WriteStack& )
   at System.Text.Json.Serialization.JsonConverter`1[[System.Object[], System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].TryWrite(Utf8JsonWriter , Object[]& , JsonSerializerOptions , WriteStack& )
   at System.Text.Json.Serialization.JsonConverter`1[[System.Object[], System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].WriteCore(Utf8JsonWriter , Object[]& , JsonSerializerOptions , WriteStack& )
   Exception_EndOfInnerExceptionStack
   at System.Text.Json.ThrowHelper.ThrowNotSupportedException(WriteStack& , NotSupportedException )
   at System.Text.Json.Serialization.JsonConverter`1[[System.Object[], System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].WriteCore(Utf8JsonWriter , Object[]& , JsonSerializerOptions , WriteStack& )
   at System.Text.Json.Serialization.Metadata.JsonTypeInfo`1[[System.Object[], System.Private.CoreLib, Version=8.0.0.0, Culture=neutral, PublicKeyToken=7cec85d7bea7798e]].Serialize(Utf8JsonWriter , Object[]& , Object )
   at System.Text.Json.JsonSerializer.WriteString[Object[]](Object[]& , JsonTypeInfo`1 )
   at System.Text.Json.JsonSerializer.Serialize[Object[]](Object[] , JsonSerializerOptions )
   at Microsoft.JSInterop.JSRuntime.InvokeAsync[IJSVoidResult](Int64 , String , CancellationToken , Object[] )
   at Microsoft.JSInterop.JSRuntime.<InvokeAsync>d__16`1[[Microsoft.JSInterop.Infrastructure.IJSVoidResult, Microsoft.JSInterop, Version=8.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60]].MoveNext()
   at Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync(IJSRuntime , String , Object[] )
   at BlazorissueWasmSerialization.Client.Pages.Home.OnButtonShowIssueClick()
   at Microsoft.AspNetCore.Components.ComponentBase.CallStateHasChangedOnAsyncCompletion(Task task)
   at Microsoft.AspNetCore.Components.RenderTree.Renderer.GetErrorHandledTask(Task , ComponentState )

```

</details>

### .NET Version

8.0.100

### Anything else?

_No response_
