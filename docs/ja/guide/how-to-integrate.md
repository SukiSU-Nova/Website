# 統合ガイド

SukiSU-Nova は GKI カーネルと非 GKI カーネルの両方に統合できます。前提条件は、起動可能なオープンソースのカーネルソースがあることです。

::: tip GKI デバイスでそのまま使いたいだけの場合
カーネルをビルドする必要はありません。マネージャーを使ってローダブルカーネルモジュール（LKM）としてインストールできます。[LKM インストール](./installation#method-1-lkm-via-the-manager-recommended)をご覧ください。
:::

一部の OEM カスタマイズでは、カーネルコードの大部分がアップストリーム Linux や ACK 由来ではなくアウトオブツリーになっています。そのため非 GKI カーネルは著しく断片化しており汎用的なビルド方法が存在せず、非 GKI カーネル向けの boot イメージは提供できません。

## ブランチの選択

| ブランチ | カーネルへのフック方法 | 主な用途 |
|----------|------------------------|----------|
| `main` | 実行時に登録される Kprobes / kretprobes および `sys_enter` システムコールトレースポイント。カーネルソースにフック呼び出しを追加する必要はありません。 | GKI カーネル、およびモジュール（LKM）または組み込みビルド |
| `builtin` | kprobes 不使用。カーネルソースから SukiSU-Nova のフックエントリポイントを呼び出します（手動フック）。 | 手動でパッチを当てるカーネル |

::: warning 非推奨・廃止オプション
`CONFIG_KSU_MANUAL_HOOK` と `CONFIG_KSU_TRACEPOINT_HOOK` はどちらのブランチにも存在しなくなりました。また、古い *Tracepoint Hook* ガイドで使われていた `ksu_trace.h` ヘッダーも削除されています。これらの有効化を指示するガイドは情報が古くなっています。
:::

## `main` ブランチ

### 要件

- `CONFIG_KPROBES=y` および `CONFIG_EXT4_FS=y`。`CONFIG_KSU` はこの両方に依存します。
- フックマネージャーは、カーネルが提供している場合に kretprobes（`CONFIG_KRETPROBES`）および `sys_enter` システムコールトレースポイント（`CONFIG_HAVE_SYSCALL_TRACEPOINTS`）を使用します。
- `CONFIG_KSU` は tristate です：組み込みにする場合は `y`、`kernelsu` モジュールとしてビルドする場合は `m` を指定します。

### カーネルソースへの追加

カーネルソースツリーのルートで次を実行します：

```sh [bash]
curl -LSs "https://git.zako.shirkneko.top:20004/shirkneko/SukiSU/raw/branch/main/kernel/setup.sh" | bash -s builtin-new
```

このスクリプトはリポジトリをクローンし、`drivers/kernelsu` へリンクして `drivers/Makefile` と `drivers/Kconfig` にエントリを追加します。元に戻す場合は `--cleanup` を使用してください。

### オプション設定

| オプション | 意味 |
|------------|------|
| `CONFIG_KSU_MANUAL_SU`（デフォルト `y`） | 手動 su を使用：`prctl` 経由でコマンドラインおよびアプリケーションを認可します。 |
| `CONFIG_KPM` | SukiSU KPM を有効化します。64-bit カーネルが必要で、`CONFIG_KALLSYMS` と `CONFIG_KALLSYMS_ALL` を選択します。システムの安定性に影響する場合があります。 |
| `CONFIG_KSU_DISABLE_MANAGER` | マネージャー APK の検出およびマネージャー固有の処理を無効化します。 |
| `CONFIG_KSU_DISABLE_POLICY` | アプリごとのプロファイルを無効化します。権限昇格時は常にデフォルトのフル Root プロファイルが使用されます。 |
| `CONFIG_KSU_X86_PATCH_SYSCALL_DISPATCHER` | x86_64 専用。強化された syscall dispatcher を動的にパッチし、カーネルソースパッチなしでシステムコールフックを動作させます。x86_64 LKM モードに有用です。 |
| `CONFIG_KSU_DEBUG` | デバッグモード。 |

## `builtin` ブランチ

::: info クレジット
このセクションの手動フック統合ガイドは backslashxx 氏の GitHub Issue に基づいています：[scope-minimized manual hooks v2.3](https://github.com/backslashxx/KernelSU/issues/5)。
:::

このブランチは kprobes を登録しません。カーネルソース内にフックエントリポイントの呼び出しを追加します（手動フック）。エントリポイントはブランチ内のヘッダー（`kernel/feature/sucompat.h`、`kernel/feature/kernel_umount.h`、`kernel/runtime/ksud.h` など）で宣言されています。**ビルドするブランチで正確なシグネチャを必ず確認してください**。以下のパッチは参考用です。

まず、ソースツリーにこのブランチを追加します：

```sh [bash]
curl -LSs "https://git.zako.shirkneko.top:20004/shirkneko/SukiSU/raw/branch/main/kernel/setup.sh" | bash -s builtin
```

このブランチには SUSFS オプション（`CONFIG_KSU_SUSFS` およびそのサブオプション）と、64-bit カーネルを必要とする `CONFIG_KPM` も含まれています。

### 始める前に

- 6.8+ の **非 ARM64** カーネルをビルドする場合は、あわせてこちらも適用してください：<https://github.com/backslashxx/KernelSU/issues/7>
- **ARM / ARM64** では、**`CONFIG_KSU_TAMPER_SYSCALL_TABLE=y`** を有効にすることで、execve、sys_faccessat、sys_newfstatat、sys_newfstat ret_hook、sys_reboot、slow_avc_audit のフックをスキップできます。**その場合は、これらの列挙されたフックを必ず削除してください！**
- **4.19+ ARM64** では、代わりに **`CONFIG_KSU_HACK_ARM64_BRANCH_LINK=y`** を試して、以下の手動フックを**すべてスキップ**することもできます。
- カーネルによってバックポートされている内容が異なります。お使いのカーネルに実際に存在する関数に一致するパッチを選択してください。

### 🟢 execve フック

- sucompat 用です。
- カーネルバージョンに合うものを **1 つ選択**してください。
- バックポートが含まれるカーネルでは、実際のカーネル実装に一致するフックを使用してください。
- カスタム ROM 上で selinuxfs の adb root を隠したい場合は、[この KSU モジュール](https://github.com/backslashxx/small_module)をインストールしてください。

**3.19+（`do_execveat_common` 経由）：**

```patch
--- a/fs/exec.c
+++ b/fs/exec.c
@@ -1728,6 +1728,11 @@
/*
 * sys_execve() executes a new program.
 */
static int do_execveat_common(int fd, struct filename *filename,
			struct user_arg_ptr argv,
			struct user_arg_ptr envp,
			int flags)
{
	char *pathbuf = NULL;
	struct linux_binprm *bprm;
	struct file *file;
	struct files_struct *displaced;
	int retval;

+#ifdef CONFIG_KSU
+	extern int ksu_handle_execveat(int *, struct filename **, void *, void *, int *);
+	ksu_handle_execveat(&fd, &filename, &argv, &envp, &flags);
+#endif
+
	if (IS_ERR(filename))
		return PTR_ERR(filename);
```

**3.19+（`__do_execve_file` 経由）：**

```patch
--- fs/exec.c
+++ fs/exec.c
@@ -1921,11 +1921,20 @@

/*
 * sys_execve() executes a new program.
 */
static int __do_execve_file(int fd, struct filename *filename,
			struct user_arg_ptr argv,
			struct user_arg_ptr envp,
			int flags, struct file *file)
{
	char *pathbuf = NULL;
	struct linux_binprm *bprm;
	struct files_struct *displaced;
	int retval;

+#ifdef CONFIG_KSU
+	extern int ksu_handle_execveat(int *, struct filename **, void *, void *, int *);
+	ksu_handle_execveat(&fd, &filename, &argv, &envp, &flags);
+#endif
+
	if (IS_ERR(filename))
		return PTR_ERR(filename);
```

**3.18（`do_execve_common` 経由）：**

- 3.18 では、アップストリームの `do_execveat_common` フックを `do_execve_common` 用に転用できます。
- <= 3.18 には `sys_execveat` が存在しないため、`sys_execve` + `compat_sys_execve` をフックする代わりにこの方法が適しています。
- 注意：引数は **`struct filename *filename`** です。

```patch
--- a/fs/exec.c
+++ b/fs/exec.c
/*
 * sys_execve() executes a new program.
 */
static int do_execve_common(struct filename *filename,
				struct user_arg_ptr argv,
				struct user_arg_ptr envp)
{
	struct linux_binprm *bprm;
	struct file *file;
	struct files_struct *displaced;
	int retval;

+#ifdef CONFIG_KSU
+	extern int ksu_handle_execveat(int *, struct filename **, void *, void *, int *);
+	ksu_handle_execveat((int *)AT_FDCWD, &filename, &argv, &envp, 0);
+#endif
+
	if (IS_ERR(filename))
		return PTR_ERR(filename);
```

**3.0 - 3.10（`do_execve_common` 経由）：**

- <= 3.10 向けに、このリポジトリは `do_execve_common` 用のハンドラを提供しています。
- <= 3.18 には `sys_execveat` が存在しないため、`sys_execve` + `compat_sys_execve` をフックする代わりにこの方法が適しています。
- 注意：引数は **`const char *filename`** です。

```patch
--- a/fs/exec.c
+++ b/fs/exec.c
@@ -1615,6 +1615,12 @@
/*
 * sys_execve() executes a new program.
 */
static int do_execve_common(const char *filename,
				struct user_arg_ptr argv,
				struct user_arg_ptr envp)
{
	struct linux_binprm *bprm;
	struct file *file;
	struct files_struct *displaced;
	bool clear_in_exec;
	int retval;
	const struct cred *cred = current_cred();

+#ifdef CONFIG_KSU
+	extern int ksu_legacy_execve_sucompat(const char **, void *, void *);
+	ksu_legacy_execve_sucompat(&filename, (void *)&argv, (void *)&envp);
+#endif
+
	/*
	 * We move the actual failure in case of RLIMIT_NPROC excess from
	 * set*uid() to execve() because too many poorly written programs
```

### 🟢 sys_faccessat フック

- sucompat 用です。
- 元のガイドに由来します。
- スコープ最小化のため、`do_faccessat` がある場合でも **`sys_faccessat`** をフックしてください。
- バックポートが含まれるカーネルでは、実際のカーネル実装に一致するフックを使用してください。

**5.10+：**

```patch
--- a/fs/open.c
+++ b/fs/open.c
@@ -483,6 +483,10 @@

SYSCALL_DEFINE3(faccessat, int, dfd, const char __user *, filename, int, mode)
{
+#ifdef CONFIG_KSU
+	extern int ksu_handle_faccessat(int *, const char __user **, int *, int *);
+	ksu_handle_faccessat(&dfd, &filename, &mode, NULL);
+#endif
	return do_faccessat(dfd, filename, mode, 0);
}
```

**4.19+：**

```patch
--- a/fs/open.c
+++ b/fs/open.c
@@ -483,6 +483,10 @@

SYSCALL_DEFINE3(faccessat, int, dfd, const char __user *, filename, int, mode)
{
+#ifdef CONFIG_KSU
+	extern int ksu_handle_faccessat(int *, const char __user **, int *, int *);
+	ksu_handle_faccessat(&dfd, &filename, &mode, NULL);
+#endif
	return do_faccessat(dfd, filename, mode);
}
```

**4.14 以前：**

```patch
--- a/fs/open.c
+++ b/fs/open.c
/*
 * access() needs to use the real uid/gid, not the effective uid/gid.
 * We do this by temporarily clearing all FS-related capabilities and
 * switching the fsuid/fsgid around to the real ones.
 */
SYSCALL_DEFINE3(faccessat, int, dfd, const char __user *, filename, int, mode)
{
	const struct cred *old_cred;
	struct cred *override_cred;
	struct path path;
	struct inode *inode;
	int res;
	unsigned int lookup_flags = LOOKUP_FOLLOW;

+#ifdef CONFIG_KSU
+	extern int ksu_handle_faccessat(int *, const char __user **, int *, int *);
+	ksu_handle_faccessat(&dfd, &filename, &mode, NULL);
+#endif
+
	if (mode & ~S_IRWXO)	/* where's F_OK, X_OK, W_OK, R_OK? */
		return -EINVAL;
```

### 🟢 sys_newfstatat フック

- sucompat 用です。
- スコープが最小化されています。
- 現在は `vfs_statx()` ではなく **`sys_newfstatat`** をフックする必要があります。
- 32-bit su が必要な場合は、任意で **`sys_fstatat64`** もフックしてください。

```patch
--- a/fs/stat.c
+++ b/fs/stat.c
@@ -371,6 +371,11 @@
#if !defined(__ARCH_WANT_STAT64) || defined(__ARCH_WANT_SYS_NEWFSTATAT)
SYSCALL_DEFINE4(newfstatat, int, dfd, const char __user *, filename,
		struct stat __user *, statbuf, int, flag)
{
	struct kstat stat;
	int error;

+#ifdef CONFIG_KSU
+	extern int ksu_handle_stat(int *, const char __user **, int *);
+	ksu_handle_stat(&dfd, &filename, &flag);
+#endif
+
	error = vfs_fstatat(dfd, filename, &stat, flag);
	if (error)
		return error;
	return cp_new_stat(&stat, statbuf);
}
#endif

@@ -515,6 +520,11 @@
SYSCALL_DEFINE4(fstatat64, int, dfd, const char __user *, filename,
		struct stat64 __user *, statbuf, int, flag)
{
	struct kstat stat;
	int error;

+#ifdef CONFIG_KSU // for 32-bit
+	extern int ksu_handle_stat(int *, const char __user **, int *);
+	ksu_handle_stat(&dfd, &filename, &flag);
+#endif
+
	error = vfs_fstatat(dfd, filename, &stat, flag);
	if (error)
		return error;
	return cp_new_stat64(&stat, statbuf);
}
```

### 🟢 sys_newfstat ret_hook

- Android 16 以降で必要です。
- アップストリームの [kernel: ksud: Refine rc injection, fix issue of Android Canary 2601](https://github.com/tiann/KernelSU/commit/df640917d11dd0eff1b34ea53ec3c0dc49667002) で導入されました。
- `fs/stat.c` で `newfstat` が戻る直前にフックしてください。
- 32-bit や 32-on-64 向けに任意で **`sys_fstat64`** をフックすることもできます（通常は不要です）。
- **`#pragma GCC`** について心配する必要はありません。Clang でも動作します。

```patch
diff --git a/fs/stat.c b/fs/stat.c
--- a/fs/stat.c
+++ b/fs/stat.c
@@ -386,6 +392,14 @@
SYSCALL_DEFINE2(newfstat, unsigned int, fd, struct stat __user *, statbuf)
{
	struct kstat stat;
	int error = vfs_fstat(fd, &stat);

	if (!error)
		error = cp_new_stat(&stat, statbuf);

+#if defined(CONFIG_KSU) && !defined(CONFIG_KSU_KPROBES_KSUD)
+#pragma GCC diagnostic push
+#pragma GCC diagnostic ignored "-Wdeclaration-after-statement"
+	extern void ksu_handle_newfstat_ret(unsigned int *, struct stat __user **);
+	ksu_handle_newfstat_ret(&fd, &statbuf);
+#pragma GCC diagnostic pop
+#endif
+
	return error;
}

@@ -506,6 +520,14 @@
SYSCALL_DEFINE2(fstat64, unsigned long, fd, struct stat64 __user *, statbuf)
{
	struct kstat stat;
	int error = vfs_fstat(fd, &stat);

	if (!error)
		error = cp_new_stat64(&stat, statbuf);

+#if defined(CONFIG_KSU) && !defined(CONFIG_KSU_KPROBES_KSUD) // for 32-bit
+#pragma GCC diagnostic push
+#pragma GCC diagnostic ignored "-Wdeclaration-after-statement"
+	extern void ksu_handle_fstat64_ret(unsigned long *, struct stat64 __user **);
+	ksu_handle_fstat64_ret(&fd, &statbuf);
+#pragma GCC diagnostic pop
+#endif
+
	return error;
}
```

### 🟢 sys_reboot フック

- 12143 で導入された新しい KernelSU supercall に必要です。
- `sys_reboot` の先頭エントリで直接フックしてください。

**3.18+（`kernel/reboot.c`）：**

```patch
--- a/kernel/reboot.c
+++ b/kernel/reboot.c
@@ -284,6 +284,11 @@
SYSCALL_DEFINE4(reboot, int, magic1, int, magic2, unsigned int, cmd,
		void __user *, arg)
{
	struct pid_namespace *pid_ns = task_active_pid_ns(current);
	char buffer[256];
	int ret = 0;

+#if defined(CONFIG_KSU) && !defined(CONFIG_KSU_KPROBES_KSUD)
+	extern int ksu_handle_sys_reboot(int, int, unsigned int, void __user **);
+	ksu_handle_sys_reboot(magic1, magic2, cmd, &arg);
+#endif
+
	/* We only trust the superuser with rebooting the system. */
	if (!ns_capable(pid_ns->user_ns, CAP_SYS_BOOT))
		return -EPERM;
```

**3.0 ~ 3.10（`kernel/sys.c`）：**

```patch
--- a/kernel/sys.c
+++ b/kernel/sys.c
@@ -463,6 +463,11 @@
 *
 * reboot doesn't sync: do that yourself before calling this.
 */
SYSCALL_DEFINE4(reboot, int, magic1, int, magic2, unsigned int, cmd,
		void __user *, arg)
{
	struct pid_namespace *pid_ns = task_active_pid_ns(current);
	char buffer[256];
	int ret = 0;

+#if defined(CONFIG_KSU) && !defined(CONFIG_KSU_KPROBES_KSUD)
+	extern int ksu_handle_sys_reboot(int, int, unsigned int, void __user **);
+	ksu_handle_sys_reboot(magic1, magic2, cmd, &arg);
+#endif
+
	/* We only trust the superuser with rebooting the system. */
	if (!ns_capable(pid_ns->user_ns, CAP_SYS_BOOT))
		return -EPERM;
```

### 🟡 policy_rwlock

- **任意**です。あると望ましいですが**必須ではありません**。
- 主に 3.0 ~ 4.9、場合によっては 4.14 向けです。
- ただしほとんどの 4.14 カーネルでは不要です（ACK により `selinux_state` がバックポートされているため）。
- 見つからない場合や手間を省きたい場合は気にする必要はありません。
- 基本的には `policy_rwlock` 定義から `static` を削除するだけです。

```patch
--- a/security/selinux/ss/services.c
+++ b/security/selinux/ss/services.c
@@ -74,7 +74,7 @@
 int selinux_android_netlink_route;
 int selinux_policycap_netpeer;
 int selinux_policycap_openperm;

-static DEFINE_RWLOCK(policy_rwlock);
+DEFINE_RWLOCK(policy_rwlock);

 static struct sidtab sidtab;
 struct policydb policydb;
```

### 🟡 slow_avc_audit

- **任意**です。
- **ARM64 では不要**です。
- 主に ARM32 向けであり、ユーザー空間で処理することも可能です。
- 標準の KSU 機能ではありませんが、必要な場合のために記載しています。

```patch
--- a/security/selinux/avc.c
+++ b/security/selinux/avc.c
/* This is the slow part of avc audit with big stack footprint */
noinline int slow_avc_audit(struct selinux_state *state,
			u32 ssid, u32 tsid, u16 tclass,
			u32 requested, u32 audited, u32 denied, int result,
			struct common_audit_data *a,
			unsigned int flags)
{
	struct common_audit_data stack_data;
	struct selinux_audit_data sad;

+#if defined(CONFIG_KSU) && !defined(CONFIG_KPROBES)
+	extern void ksu_slow_avc_audit(u32 *);
+	ksu_slow_avc_audit(&tsid);
+#endif
	if (!a) {
		a = &stack_data;
		a->type = LSM_AUDIT_DATA_NONE;
```

### 最適化のヒント

- 最近の **Clang** と **FULL LTO** を使用している場合、オプティマイザにフックをインライン化するヒントを与えることができます。
- 確実に効くとは限りませんが、機能すればより高い最適化が得られます：

```patch
 #ifdef CONFIG_KSU
 	extern int ksu_handle_execveat(int *, struct filename **, void *, void *, int *);
-	ksu_handle_execveat(&fd, &filename, &argv, &envp, &flags);
+	[[clang::always_inline]] ksu_handle_execveat(&fd, &filename, &argv, &envp, &flags);
 #endif
```

```patch
 #ifdef CONFIG_KSU
 	extern int ksu_handle_faccessat(int *, const char __user **, int *, int *);
-	ksu_handle_faccessat(&dfd, &filename, &mode, NULL);
+	[[clang::always_inline]] ksu_handle_faccessat(&dfd, &filename, &mode, NULL);
 #endif
```

```patch
 #ifdef CONFIG_KSU
 	extern int ksu_handle_stat(int *, const char __user **, int *);
-	ksu_handle_stat(&dfd, &filename, &flag);
+	[[clang::always_inline]] ksu_handle_stat(&dfd, &filename, &flag);
 #endif
```

### Kconfig リファレンス

以下の Kconfig は backslashxx 氏の KernelSU リポジトリ（上記ガイドと同じソース）に由来するもので、参考として掲載しています：

```kconfig
menu "KernelSU"

config KSU
	tristate "KernelSU function support"
	depends on !CPU_BIG_ENDIAN
	depends on SECURITY_SELINUX
	select SECCOMP
	default y
	help
	  Enable kernel-level root privileges on Android System.

config KSU_KPROBES_KSUD
	bool "Enable dynamic kprobes for early boot hooks"
	depends on KSU
	depends on !KSU_TAMPER_SYSCALL_TABLE
	depends on !KSU_HACK_ARM64_BRANCH_LINK
	depends on KPROBES && KRETPROBES
	depends on DEPRECATED
	default n
	help
	  [deprecated] Use dynamic hooks via kprobes for functions only
	  on early boot. Hooks are unregistered at boot complete
	  to reduce overhead.

config KSU_HACK_ARM64_BRANCH_LINK
	bool "inline call-site hijacking (ARM64 bl insn)"
	depends on KSU
	depends on ARM64 && KALLSYMS
	depends on !KSU_TAMPER_SYSCALL_TABLE
	default y if KSU = m
	default n
	help
	  Attempts to provide sucompat by scanning kernel text and overwriting caller's
	  branch insn ('b' or 'bl') directly to our hooks, bypassing trampoline overhead
	  and CFI fuckups. Inspired by ARM64 optprobes.
	  Successful patching relies on symbol stability, so hooking success will
	  vary across different compliers, optimization, tweaks and backports.
	  When patching fails, it keeps syscall table hook for the target symbol.
	  Verified on 3.10 ~ 7.1. Recommended for 4.19+.

config KSU_TAMPER_SYSCALL_TABLE
	bool "tamper sys_call_table for sucompat + sys_reboot"
	depends on KSU
	depends on (ARM || ARM64)
	default n
	help
	  Use a high-performance syscall table hijacking method demonstrated on zx2c4's
	  kernel-assisted-superuser. Replaces sys_reboot, sys_execve, sys_newfstatat,
	  sys_faccessat, sys_newfstat_ret.
	  As syscalls are called directly, it avoids indirect branch (blr) overhead and
	  complies with Clang CFI.
	  Verified on 3.0 ~ 7.1. Recommended for 3.0 ~ 4.14.

config KSU_DISABLE_32BIT_HOOKS
	bool "disable 32-bit symbol hooking"
	depends on KSU
	depends on 64BIT && COMPAT
	depends on (KSU_TAMPER_SYSCALL_TABLE || KSU_HACK_ARM64_BRANCH_LINK)
	default n
	help
	  Skips hooking 32-bit syscalls (compat_sys_call_table). Mostly useful for zygote64-only devices.
	  Author assumes you know what you're doing, __builtin_assume(!!&user_brain).

config KSU_LSM_SECURITY_HOOKS
	bool "Use LSM security hooks"
	depends on KSU
	default y
	help
	  Disabling this is mostly useful for non-ARM64 with kernel > 6.8.
	  Make sure to implement manual hooks on security/security.c.

config KSU_FEATURE_SULOG
	bool "KernelSU SU Logging feature"
	depends on KSU
	default y
	help
	  Build KernelSU's SU Log.

config KSU_FEATURE_ADBROOT
	bool "KernelSU ADB Root feature"
	depends on KSU
	default y
	help
	  Build KernelSU's adb root feature.

config KSU_HOSTSREDIRECT
	bool "/system/etc/hosts redirection"
	depends on KSU
	depends on (ARM || ARM64)
	default n
	help
	  Kernel-level sys_openat fd redirect targeting /system/etc/hosts to /data/adb/hosts.
	  Allows hosts file modification without mounting hosts file directly, this is meant to
	  be used with bindhosts mode 3. If /data/adb/hosts does NOT exist at boot, the syscall
	  hook is automatically skipped. See like ZN-hostsredirect / APatch hosts file redirect.
	  If unsure, say n.

config KSU_ENABLE_FULL_UID_CHECKS
	bool "perform full uid checks (reduced performance)"
	depends on KSU
	default n
	help
	  Validate every non-root UID against the allowlist. When disabled, only shell (2000)
	  is checked. Recommended for permissive selinux / seccomp disabled scenarios.
	  Enable for extended security; keep disabled for performance.

config KSU_THRONE_TRACKER_ALWAYS_THREADED
	bool "Always run throne tracker in a kthread"
	depends on KSU
	default n
	help
	  Enable this option to run throne tracker in a kthread for the first
	  run, which happens at boot time / decryption stage. This can decrease
	  boot time, but can cause crowning failure on some FDE/FBEv1 setups.
	  If unsure, say n.

config KSU_NOPRINTK
	bool "disable ALL dmesg logging"
	depends on KSU
	depends on !KSU_DEBUG
	default n
	help
	  Disable KernelSU dmesg output.

config KSU_SHELL_HAS_SU_ALWAYS
	bool "give shell su"
	depends on KSU
	default n
	help
	  Always allow shell (2000) for su. Can speedup sucompat gate as a side-effect.

config KSU_DEBUG
	bool "KernelSU debug mode"
	depends on KSU
	default n
	help
	  Enable KernelSU debug mode.

config KSU_HEURISTIC_IN_TREE_BUILD
	bool "heuristic. do NOT touch"
	depends on KSU
	default y
	help
	  Note: if youre building out-of-tree, configure by editing ksu.c #defines instead.

config KPM
    bool "Enable SukiSU KPM"
    depends on KSU && 64BIT
    default n
    help
      Enabling this option will activate the KPM feature of SukiSU.
      This option is suitable for scenarios where you need to force KPM to be enabled.
      but it may affect system stability.
    select KALLSYMS
    select KALLSYMS_ALL

endmenu
```
