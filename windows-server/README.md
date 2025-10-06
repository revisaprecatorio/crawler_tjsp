# 🪟 Migração para Windows Server - Crawler TJSP

**Data de Início:** 2025-10-04
**Status:** 🟡 Em Teste (Fase 6)
**Servidor:** Contabo Cloud VPS 10 (62.171.143.88)
**Última Atualização:** 2025-10-05

---

## 📋 Especificações do Servidor

### Hardware
- **CPU:** 3 vCPU Cores
- **RAM:** 8 GB
- **Storage:** 75 GB NVMe + 150 GB SSD
- **Região:** European Union
- **Backup:** Auto Backup habilitado
- **Transferência:** 32 TB Out + Unlimited In

### Sistema Operacional
- **OS:** Windows Server 2016 Datacenter
- **Snapshot:** 1 disponível

### Custo Estimado
- **Mensal:** ~€9-15 (R$50-80)
- **Anual:** ~€108-180 (R$600-1.000)

---

## 🎯 Objetivo da Migração

**Resolver o bloqueio do Native Messaging Protocol** que impede a autenticação via certificado digital no ambiente Linux headless.

### Por Que Windows Server?

✅ **Native Messaging funciona nativamente**
- ChromeDriver executa Chrome em modo desktop
- Web Signer se comunica com extensão sem restrições
- Selenium mantém controle total do browser

✅ **Solução já validada**
- Evidências do DEPLOY_TRACKING.md mostram que funciona via RDP
- Login manual bem-sucedido (Deploy #30)
- Mesma arquitetura do ambiente de desenvolvimento

✅ **Custo-benefício otimizado**
- Tier 1: Solução definitiva e de baixo custo
- Sem dependências de serviços terceiros
- Total controle sobre infraestrutura

---

## 📁 Estrutura deste Diretório

```
windows-server/
├── README.md                          # Este arquivo (Status geral)
├── DEPLOYMENT_PLAN.md                 # Plano detalhado de deploy
├── MIGRATION_CHECKLIST.md             # Checklist de migração (atualizado)
├── CHROME_PROFILE_FIX.md              # ✨ Documentação da correção crítica
├── TESTE_FASE_5.md                    # ✨ NOVO: Guia completo de testes
├── BACKUP_GUIDE.md                    # 💾 Guia completo de backup (8 etapas)
├── RESTORE_GUIDE.md                   # 🔄 Guia de restore (snapshot + manual)
├── QUICK_BACKUP.md                    # ⚡ Guia executivo de 5 passos (30-45 min)
├── CREDENTIALS.md                     # Credenciais (protegido por .gitignore)
├── QUICKSTART.md                      # Guia rápido de execução
├── EXECUTE_NOW.md                     # Instruções de execução imediata
├── setup/
│   ├── 01_initial_server_setup.md    # Configuração inicial (RDP, SSH, firewall)
│   ├── 02_python_installation.md     # Python 3.12 + pip + virtualenv
│   ├── 03_chrome_websigner.md        # Chrome + Web Signer + Certificado
│   ├── 04_postgresql.md              # PostgreSQL 15 instalação e setup
│   └── 05_crawler_deployment.md      # Deploy do crawler + worker
├── scripts/
│   ├── setup-simple.ps1              # Script instalação automatizada
│   ├── backup_complete_system.ps1    # 💾 Backup automático completo
│   ├── export_certificado.ps1        # 🔐 Export certificado digital
│   ├── test_authentication.py        # ✨ TESTE #1: Login com certificado
│   ├── test_direct_process_access.py # ✨ NOVO: TESTE #2: Acesso direto
│   └── start_services.ps1            # Iniciar crawler + orchestrator
└── docs/
    ├── windows_vs_linux.md           # Comparativo de arquitetura
    ├── troubleshooting.md            # Troubleshooting específico Windows
    └── security_hardening.md         # Hardening de segurança

```

---

## 🚀 Roadmap de Implementação

### Fase 1: Preparação do Servidor ✅ (1-2 horas)
- [x] Receber credenciais de acesso da Contabo
- [x] Configurar RDP (Remote Desktop Protocol)
- [x] Configurar SSH (OpenSSH Server v9.5.0.0p1-Beta)
- [x] Configurar Windows Firewall (porta 22 liberada)
- [ ] Criar snapshot inicial
- [ ] Habilitar Auto Backup

### Fase 2: Instalação de Dependências ✅ (2-3 horas)
- [x] Instalar Python 3.12.3
- [x] Instalar Git para Windows (via TLS 1.2)
- [x] Instalar Google Chrome (v131.0.6778.86)
- [x] Instalar ChromeDriver (C:\chromedriver\)
- [x] Instalar Web Signer (Softplan)
- [ ] Instalar PostgreSQL 15 (aguardando decisão)
- [x] Configurar variáveis de ambiente

### Fase 3: Configuração de Certificado ✅ (30 min)
- [x] Transferir certificado A1 (.pfx) via SCP
- [x] Importar certificado no Windows Certificate Store
- [x] Configurar Web Signer com certificado
- [x] Validar conexão extensão ↔ Web Signer
- [x] Teste manual de login bem-sucedido

### Fase 4: Deploy do Crawler ✅ (1-2 horas)
- [x] Clonar repositório via Git
- [x] Criar virtualenv Python (.venv)
- [x] Instalar dependências (requirements.txt)
- [x] Configurar .env com certificado e Chrome
- [x] **CORREÇÃO CRÍTICA:** Ajustar perfil Chrome para usar padrão
- [x] Script de teste criado (test_authentication.py)

### Fase 5: Teste de Autenticação 🟡 (30 min)
- [x] **DESCOBERTA:** Chrome sincronizado com perfil Google
- [x] **FIX APLICADO:** Remover --user-data-dir do Selenium
- [ ] ⏳ Executar teste de login com certificado (próximo passo)
- [ ] Validar Native Messaging funcionando
- [ ] Screenshot de login bem-sucedido
- [ ] Log de autenticação capturado

### Fase 6: Configuração do Worker ⬜ (1 hora)
- [ ] Configurar orchestrator_subprocess.py
- [ ] Criar Windows Service para orchestrator
- [ ] Configurar auto-start no boot
- [ ] Testar processamento de fila

### Fase 7: Produção e Monitoramento ⬜ (1 hora)
- [ ] Configurar logs (rotação automática)
- [ ] Configurar alertas (email/webhook)
- [ ] Documentar procedimentos de manutenção
- [ ] Realizar backup completo
- [ ] Iniciar operação em produção

---

## ⚠️ Riscos e Mitigações

| Risco | Probabilidade | Impacto | Mitigação |
|-------|--------------|---------|-----------|
| Licença Windows Server expirar | Baixa | Alto | Contabo gerencia licenciamento |
| Performance inferior ao Linux | Média | Médio | Monitorar uso de recursos, escalar se necessário |
| Custos de manutenção maiores | Baixa | Médio | Auto-backup reduz custos operacionais |
| Vulnerabilidades de segurança | Média | Alto | Hardening + Windows Updates automáticos |
| Incompatibilidade de bibliotecas Python | Baixa | Médio | Selenium e psycopg2 têm suporte oficial Windows |

---

## 📊 Comparativo: Linux vs Windows

| Aspecto | Linux (Atual) | Windows Server (Proposto) |
|---------|---------------|---------------------------|
| Native Messaging | ❌ Não funciona | ✅ Funciona |
| Custo Mensal | €5-10 | €9-15 |
| Manutenção | Complexa | Moderada |
| Debugging | Difícil (headless) | Fácil (RDP visual) |
| Estabilidade Selenium | Baixa | Alta |
| Suporte Web Signer | Não oficial | Oficial |
| Auto-scaling | Fácil | Moderado |

---

## 📚 Referências

- [DIAGNOSTIC_REPORT.md](../DIAGNOSTIC_REPORT.md) - Análise completa das 30 tentativas
- [DEPLOY_TRACKING.md](../DEPLOY_TRACKING.md) - Histórico detalhado de deploys
- [Windows Server 2016 Documentation](https://learn.microsoft.com/en-us/windows-server/)
- [Selenium on Windows](https://www.selenium.dev/documentation/webdriver/getting_started/install_drivers/)
- [Python on Windows](https://docs.python.org/3/using/windows.html)

---

## 🎯 Descoberta Crítica: Chrome Profile Fix

### Problema Identificado

Selenium abria Chrome sem extensão Web Signer instalada, impedindo autenticação com certificado digital.

### Causa Raiz

- Chrome sincronizado com Google Account (`revisa.precatorio@gmail.com`)
- Script usava `--user-data-dir=C:\temp\chrome-profile-test`
- Isso criava perfil novo/isolado **sem extensões da nuvem**

### Solução Aplicada

✅ **Remover argumento `--user-data-dir` do Selenium**

Isso permite que Chrome use perfil padrão (sincronizado) onde Web Signer está instalado.

**Documentação completa:** [CHROME_PROFILE_FIX.md](CHROME_PROFILE_FIX.md)

---

## 🤝 Próximos Passos

1. ✅ **Credenciais recebidas e ambiente configurado**
2. ✅ **Fases 1-4 concluídas**
3. ✅ **Correção crítica de perfil Chrome aplicada**
4. 💾 **NOVO: Backup completo antes de upgrade para WS2025**
   - Consultar: `QUICK_BACKUP.md` (guia de 5 passos, 30-45 min)
   - Ou: `BACKUP_GUIDE.md` (guia completo detalhado)
   - Scripts prontos: `backup_complete_system.ps1` e `export_certificado.ps1`
5. ⏳ **Executar teste de autenticação no servidor** (Fase 5)
6. ⏳ **Validar Native Messaging Protocol funcionando**
7. ⏳ **Avançar para Fase 6 (Worker) e Fase 7 (Produção)**

---

## 💾 Backup e Upgrade (NOVO)

**Se for fazer upgrade para Windows Server 2025:**

### Guia Rápido (30-45 min):
```markdown
1. Ler: QUICK_BACKUP.md
2. Criar snapshot Contabo (CRÍTICO!)
3. Executar: .\scripts\backup_complete_system.ps1
4. Transferir ZIP para local seguro
5. Validar hash MD5
6. ✅ Pronto para upgrade!
```

### Se precisar restaurar:
```markdown
- Método 1: Restore via snapshot Contabo (10-20 min)
- Método 2: Restore manual via backup ZIP (2-4 horas)
- Consultar: RESTORE_GUIDE.md
```

### Instalação Limpa do ZERO (Recomendado):
```markdown
- Guia completo: FRESH_INSTALL_WS2025.md
- Tempo: 3-4 horas
- Zero heranças, sistema 100% limpo
- Todas as 8 fases documentadas passo a passo
```

---

**Última atualização:** 2025-10-06
**Responsável:** Persival Balleste
**Status:** 🟡 Fase 5 em andamento + 💾 Sistema de backup implementado
