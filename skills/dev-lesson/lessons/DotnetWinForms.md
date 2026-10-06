# .NET WinForms 桌面 GUI 工程经验

适用：用 C#/.NET 写 WinForms 桌面/托盘/窗体工具（常含后台定时循环 + 界面刷新）。与「本地模型翻译」等业务无关，是纯工程通用点。

## 一、跨线程更新控件必须 Invoke

后台线程 / Timer / 异步回调里拿到结果后更新控件，先判断 `InvokeRequired`，为 true 时用 `BeginInvoke`/`Invoke` 切回 UI 线程再改，否则抛 InvalidOperationException「线程间操作无效：从不是创建控件的线程访问它」。

后台定时循环（截屏→模型→显示这类）的完成回调最容易漏：回调跑在非 UI 线程，直接 `label.Text = result` 就崩，且异步时序下断点难复现，容易先怀疑是别处的状态问题。

```csharp
private void OnResult(string text)
{
    if (InvokeRequired) { BeginInvoke(new Action<string>(OnResult), text); return; }
    label.Text = text;
}
```

## 二、程序运行中构建会因文件锁失败

WinForms 程序运行中锁住输出的 exe/dll，`dotnet build` 报「文件正由另一进程使用」而失败。先 `taskkill /F /IM 程序名.exe` 结束进程再 build。`-p:UseAppHost=false` 让构建不生成 exe（避免 exe 被锁），但 dll 仍可能被运行进程锁住，根治仍是杀进程。

## 三、config 用 exe 目录，别用工作目录

读/写 config 要分清「程序工作目录 `Environment.CurrentDirectory`」与「exe 所在目录 `AppContext.BaseDirectory`」。`CurrentDirectory` 会随启动方式变（开始菜单快捷方式的「起始位置」≠ exe 目录），用它定位 config 会导致读写落到意外位置、出现「模板 + 运行时生成」两份 config，改了字段不生效。用 `AppContext.BaseDirectory` 拼 config 路径才稳定。
