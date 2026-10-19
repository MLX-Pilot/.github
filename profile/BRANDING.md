# Marca da organização

Os arquivos desta pasta são cópias dos assets oficiais do frontend de
[MLX-Pilot/MLX-Pilot](https://github.com/MLX-Pilot/MLX-Pilot/tree/main/apps/desktop-ui/ui/assets).
A [identidade Orbital Command](https://github.com/MLX-Pilot/MLX-Pilot/blob/main/docs/brand-identity.md)
define a geometria, as cores e a tipografia. A marca completa usa o símbolo
hexagonal e lettering Outfit em curvas; não depende de fontes ou serviços externos.

| Arquivo neste repositório | Origem no repositório principal | Dimensões | Uso |
| --- | --- | --- | --- |
| `profile/mlxpilot-banner.svg` | `apps/desktop-ui/ui/assets/mlxpilot-banner.svg` | 1280 × 320 | Marca completa do README; exibida em 720 × 180 |
| `profile/mlxpilot-avatar.png` | `apps/desktop-ui/ui/assets/mlxpilot-mark.png` | 1024 × 1024 | Imagem pronta para upload como avatar da organização |

## Atualizar as cópias

Atualize primeiro os assets no repositório principal, seguindo seu guia de
identidade visual. Com os checkouts `MLX-Pilot` e `.github` lado a lado, execute a
partir da raiz de `.github`:

```sh
cp ../MLX-Pilot/apps/desktop-ui/ui/assets/mlxpilot-banner.svg profile/mlxpilot-banner.svg
cp ../MLX-Pilot/apps/desktop-ui/ui/assets/mlxpilot-mark.png profile/mlxpilot-avatar.png
git diff --check
```

Preserve as proporções. Confira o banner sobre fundo claro e escuro e o avatar
em tamanho pequeno. O SVG é autocontido; o README usa a URL absoluta deste mesmo
repositório para resolver a imagem também no perfil público da organização.
`Visual_Edit.jpg` foi preservado como asset anterior.

## Atualizar o avatar no GitHub

**Versionar ou publicar o PNG não altera o avatar automaticamente.** Com uma
sessão autenticada e permissão para administrar a organização, abra as
[configurações do perfil](https://github.com/organizations/MLX-Pilot/settings/profile),
escolha **Upload new picture** e envie `profile/mlxpilot-avatar.png`. Confira o
recorte e salve. Esse upload é uma etapa separada da atualização do README.

O procedimento é descrito na
[documentação do GitHub](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/customizing-your-organizations-profile).
