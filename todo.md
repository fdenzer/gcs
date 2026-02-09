# Android Dependency Compile Plan

- [ ] 1. Pin toolchain versions first
  - [ ] Install `go 1.25.3` (matches `go.mod`)
  - [ ] Install JDK `17`
  - [ ] Install Android SDK (target platform), build-tools, and platform-tools
  - [ ] Install Android NDK (pin one version, e.g. `26.x`) and CMake
  - [ ] Export and validate env vars: `ANDROID_HOME`, `ANDROID_SDK_ROOT`, `ANDROID_NDK_HOME`, `JAVA_HOME`, `PATH`

- [ ] 2. Verify host build before Android
  - [ ] Run `./build.sh --go` from repo root
  - [ ] Fix host-side Go/module issues before Android work

- [ ] 3. Lock and prefetch Go modules
  - [ ] Run `go mod download`
  - [ ] Run `go mod verify`
  - [ ] Commit `go.mod/go.sum` changes (if any)

- [ ] 4. Compile dependency graph for Android ABIs explicitly
  - [ ] For `arm64`: `GOOS=android GOARCH=arm64 CGO_ENABLED=1 go list -deps ./...`
  - [ ] For `arm64`: `GOOS=android GOARCH=arm64 CGO_ENABLED=1 go build ./...`
  - [ ] For `amd64` (if needed): repeat list/build commands

- [ ] 5. Install/verify external packaging dependency chain
  - [ ] Ensure Unison prerequisites are installed and visible in environment
  - [ ] Ensure `upack`/packager Android requirements are configured

- [ ] 6. Run Android package build
  - [ ] Produce debug/unsigned APK or AAB first
  - [ ] Add signing only after compile path is green

- [ ] 7. Triage failures by class
  - [ ] Go compile errors: module/version/tag incompatibility
  - [ ] CGO/linker errors: NDK path/version/ABI mismatch
  - [ ] Gradle/SDK errors: missing platform/build-tools or wrong JDK
  - [ ] Signing errors: keystore/configuration

- [ ] 8. Automate as CI gate
  - [ ] Add `scripts/android-preflight.sh` for Steps 3-4
  - [ ] Add CI job to run preflight on each PR
