# 🚀 Guia de Deploy no Azure

Este guia explica como implantar o **Conversor Universal de Unidades** no Azure usando o benefício estudantil (gratuito) ou planos básicos. 

---

## ✅ Pré-requisitos
1. **[Conta Azure Estudante](https://azure.microsoft.com/pt-bc/free/students/)**  
   - $100 em créditos por 12 meses + serviços gratuitos.
2. **[.NET SDK 8.0](https://dotnet.microsoft.com/download)**  
   - Verifique com `dotnet --version`.
3. **CLI do Azure**  
   - Instale em: [https://docs.microsoft.com/pt-br/cli/azure/install-azure-cli](https://docs.microsoft.com/pt-br/cli/azure/install-azure-cli).

---

## 🛠️ Passo a Passo

### 1. **Publicar o Projeto Localmente**
```bash
dotnet publish -c Release -o ./publish
```

### 2. **Fazer Login na Azure**
```bash
az login
```

### 3. **Criar Grupo de Recursos**
```bash
az group create --name ConversorUnidades-Group --location "Brazil South"
```

### 4. **Criar Plano do App Service (Free Tier)**
```bash
az appservice plan create \
    --name ConversorUnidadesPlano \
    --resource-group ConversorUnidades-Group \
    --sku F1 \
    --is-linux
```

### 5. **Criar Web App**
```bash
az webapp create \
    --name conversor-unidades-2025 \  # ⚠️ Escolha um nome único!
    --resource-group ConversorUnidades-Group \
    --plan ConversorUnidadesPlano \
    --runtime "DOTNETCORE:8.0"
```

### 6. **Configurar Variáveis de Ambiente (Opcional)**
```bash
az webapp config appsettings set \
    --name conversor-unidades-2025 \
    --resource-group ConversorUnidades-Group \
    --settings "AdsSettings__AdClient=ca-pub-SEU_ID_ADENSE"
```

### 7. **Fazer Deploy do Código**
#### Método 1: ZIP Deploy (Recomendado)
```powershell
# Windows (PowerShell)
Compress-Archive -Path .\publish\* -DestinationPath .\deploy.zip

az webapp deployment source config-zip \
    --src .\deploy.zip \
    --name conversor-unidades-2025 \
    --resource-group ConversorUnidades-Group
```

#### Método 2: GitHub Actions
1. Conecte seu repositório ao Azure em **Deployment Center**.
2. Faça push para a branch `main`:
   ```bash
   git push origin main
   ```

---

## 🌐 Acessar o Site
**URL do Site**:  
🔗 `https://conversorunidades-2025-gratis.azurewebsites.net/`

---

## 🔍 Verificações Pós-Deploy
1. **Teste as Conversões**:  
   Acesse `/weight?value=1&from=kg&to=lb`.
2. **Valide o `ads.txt`**:  
   Acesse `https://[NOME-DA-APP]/ads.txt`.
3. **Monitore Logs**:  
   ```bash
   az webapp log tail --name conversor-unidades-2025 --resource-group ConversorUnidades-Group
   ```

---

## 🚨 Solução de Problemas
| Erro                | Causa Provável               | Solução                          |
|---------------------|------------------------------|----------------------------------|
| HTTP 500            | API Key não configurada      | Verifique as variáveis de ambiente |
| Anúncios não carregam | `ads.txt` ausente           | Adicione o arquivo em `wwwroot` |
| CSS/JS não atualizados | Cache do navegador         | `Ctrl + F5` para limpar cache   |

---

## 📈 Otimizações Recomendadas
1. **CDN Gratuito**  
   Use [Cloudflare](https://www.cloudflare.com/) para acelerar o site globalmente.
2. **SSL Automático**  
   Habilite HTTPS no Azure Portal em **SSL settings**.
3. **Backup Diário**  
   Configure em **Backup** > **Configure Backup**.

---

## 🔗 Links Úteis
- [Documentação Oficial Azure](https://learn.microsoft.com/pt-br/azure/app-service/)
- [Configurar AdSense no Site](https://github.com/YanSales/ConversorUnidades/blob/main/README.md#-configura%C3%A7%C3%A3o)
- [Políticas do AdSense](https://support.google.com/adsense/answer/48182)

---

**Pronto!** Seu conversor está no ar e pronto para monetização.  
[Voltar ao README principal](README.md) | [Reportar Problema](https://github.com/YanSales/ConversorUnidades/issues)
