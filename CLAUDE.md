@AGENTS.md

## Específico do Claude Code

- **Skills, subagentes e comandos são equipamento de máquina**, instalados globalmente em `~/.agents` e symlinkados em `~/.claude`. Este repo **não** versiona nenhum deles: a cópia global é a única (cláusula de zero redundância da org). Não vendorize.
- **Permissions e statusline** são globais, com override apenas em `.claude/settings.local.json`, que é gitignored. Este repo não versiona lista de permissões.
- **Hooks portáveis** são globais e ativados por *marker file* no repo. O portão local aqui é o `lefthook.yml` versionado; a ferramenta que o executa é equipamento da máquina, e um clone numa máquina sem ela perde capacidade por design, não por omissão.
