# Package Catalog

All packages currently published to the Kosh registry.

---

## calculus

**Version:** 0.1.0 &nbsp;|&nbsp; **Deps:** none

Numerical calculus library for the vāṇī compiler.

Provides numerical integration (trapezoidal rule, Simpson's rule) and
differentiation (finite difference) over `f64` functions.

- **Repository:** [enthusiasticgeek/vani-calculus](https://github.com/enthusiasticgeek/vani-calculus)
- **Checksum (0.1.0):** `4131d2cd…e626a3e`

```toml
[deps]
calculus = { registry = "kosh", version = "^0.1" }
```

---

## probability

**Version:** 0.1.0 &nbsp;|&nbsp; **Deps:** none

Probability and statistics library for the vāṇī compiler.

Includes common distributions (uniform, normal, binomial), descriptive
statistics (mean, variance, standard deviation), and basic sampling utilities.

- **Repository:** [enthusiasticgeek/vani-probability](https://github.com/enthusiasticgeek/vani-probability)
- **Checksum (0.1.0):** `0538b26e…26c1d1`

```toml
[deps]
probability = { registry = "kosh", version = "^0.1" }
```

---

## hello-kosh

**Version:** 0.2.0 &nbsp;|&nbsp; **Deps:** none

Minimal example package. Use this as a starting point when creating a new
kosh package or when testing your local `vanic publish` setup.

- **Checksum (0.2.0):** `73aa4fa6…38ac9`
- **Checksum (0.1.0):** `1719dcb1…3222b`

```toml
[deps]
hello-kosh = { registry = "kosh", version = "^0.2" }
```

---

*Want your package listed here? See [Publishing](publishing/README.md).*
