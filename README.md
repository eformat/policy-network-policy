# policy-network-policy

- [Common base](input/base)
- [Dev Overlay](input/overlay/dev)
- [Test Overlay](input/overlay/test)

Generate NetworkPolicy for dev+test

```bash
kustomize build --enable-helm --enable-alpha-plugins .
```

Generate Policy for dev

```bash
kustomize build --enable-helm --enable-alpha-plugins input/overlay/dev
```

Generate Policy for test

```bash
kustomize build --enable-helm --enable-alpha-plugins input/overlay/test
```
