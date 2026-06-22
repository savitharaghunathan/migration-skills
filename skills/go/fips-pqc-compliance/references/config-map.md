# Config Map

| old_property | new_property | default_changed | old_default | new_default | file_pattern | notes | source_section |
|---|---|---|---|---|---|---|---|
| GOEXPERIMENT=boringcrypto | GOFIPS140=latest | true | boringcrypto | latest | Dockerfile, Makefile, CI scripts | BoringCrypto superseded by Go 1.24+ native FIPS. Set GOFIPS140=latest at build time | BoringCrypto Migration |
| GOFIPS=1 | GOFIPS140=latest | true | 1 | latest | Dockerfile, Makefile, CI scripts | Legacy golang-fips/go setting. Replace with GOFIPS140=latest | BoringCrypto Migration |
| GODEBUG=tlskyber=1 | removed | | | | .env, Dockerfile, runtime config | Legacy Kyber setting replaced by default ML-KEM in Go 1.24+. Remove — ML-KEM is on by default | PQC Configuration |
| GODEBUG=tlsmlkem=0 | removed | | | | .env, Dockerfile, runtime config | Disables PQC key exchange in TLS. Remove to enable ML-KEM hybrid key exchange | PQC Configuration |
| InsecureSkipVerify: true | InsecureSkipVerify: false | true | true | false | *.go (tls.Config) | Disables TLS certificate verification (CWE-295). Remove or set to false | TLS Configuration |
| MinVersion: tls.VersionTLS10 | MinVersion: tls.VersionTLS12 | true | TLS 1.0 | TLS 1.2 | *.go (tls.Config) | TLS 1.0 deprecated (RFC 8996). Minimum TLS 1.2 required (SP 800-52) | TLS Configuration |
| MinVersion: tls.VersionTLS11 | MinVersion: tls.VersionTLS12 | true | TLS 1.1 | TLS 1.2 | *.go (tls.Config) | TLS 1.1 deprecated (RFC 8996). Minimum TLS 1.2 required (SP 800-52) | TLS Configuration |
| MaxVersion: tls.VersionTLS12 | removed | | | | *.go (tls.Config) | Blocking TLS 1.3 prevents PQC key exchange (ML-KEM). Remove MaxVersion cap | TLS Configuration |
| CipherSuites: []uint16{...} | removed | | | | *.go (tls.Config) | Hardcoded cipher suites prevent automatic FIPS compliance. Remove to use Go defaults | TLS Configuration |
| CurvePreferences: []tls.CurveID{...} | removed | | | | *.go (tls.Config) | Restricting curves may exclude ML-KEM hybrid (X25519MLKEM768). Remove to allow PQC | TLS Configuration |
| ServerName: "" | ServerName: "hostname" | true | (empty) | (actual hostname) | *.go (tls.Config) | Empty ServerName skips hostname verification. Set to the expected server hostname | TLS Configuration |
| grpc.WithInsecure() | grpc.WithTransportCredentials(credentials.NewTLS(tlsConfig)) | | | | *.go (gRPC client) | Unencrypted gRPC (CWE-319). Use TLS transport credentials | TLS Configuration |
| rest.TLSClientConfig.Insecure = true | rest.TLSClientConfig.Insecure = false | true | true | false | *.go (Kubernetes client) | Disables TLS verification for Kubernetes API. Remove or set to false | TLS Configuration |
| sslmode=disable | sslmode=verify-full | true | disable | verify-full | *.go (database DSN) | Database connection without TLS. Use sslmode=verify-full | TLS Configuration |
| tls=false | tls=true | true | false | true | *.go (database DSN) | Database connection without TLS. Enable TLS | TLS Configuration |
| http:// | https:// | | | | *.go (URL strings) | Plain HTTP for service communication. Use HTTPS with TLS 1.2+ | TLS Configuration |
