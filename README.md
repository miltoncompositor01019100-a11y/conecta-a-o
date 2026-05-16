Bash 
#!/bin/bash 
# 
====================================================================
========== 
# Script: monitor_comunidade.sh 
# Descrição: Monitora o uptime da rede e recursos do servidor 
comunitário. 
# Desenvolvido para o Projeto Conecta-Ação 
# 
====================================================================
========== 
# Configurações 
LOG_DIR="/var/log/conecta_acao" 
LOG_FILE="$LOG_DIR/status_rede.log" 
IP_TESTE="8.8.8.8" # DNS do Google para teste de ping 
LIMITE_DISCO=80    
# Alerta se o uso do disco passar de 80% 
# Garantir que o diretório de logs existe 
if [ ! -d "$LOG_DIR" ]; then 
sudo mkdir -p "$LOG_DIR" 
sudo chmod 755 "$LOG_DIR" 
fi 
echo "==================================================" >> 
"$LOG_FILE" 
echo "Iniciando Verificação: $(date '+%Y-%m-%d %H:%M:%S')" >> 
"$LOG_FILE" 
echo "==================================================" >> 
"$LOG_FILE" 
# 1. Teste de Conectividade (Ping) 
if ping -c 3 "$IP_TESTE" > /dev/null 2>&1; then 
echo "[OK] - Conectividade com a Internet: ATIVA" >> "$LOG_FILE" 
else 
echo "[ALERTA] - Conectividade com a Internet: QUEDA DETECTADA" 
>> "$LOG_FILE" 
fi 
# 2. Verificação de Armazenamento (Disco) 
USO_DISCO=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//') 
if [ "$USO_DISCO" -gt "$LIMITE_DISCO" ]; then 
echo "[ALERTA] - Armazenamento crítico! Uso atual: $USO_DISCO%" 
>> "$LOG_FILE" 
else 
echo "[OK] - Espaço em disco: Saudável ($USO_DISCO%)" >> 
"$LOG_FILE" 
fi 
# 3. Consumo de Memória RAM 
MEM_LIVRE=$(free -m | awk 'NR==2 {print $7}') 
echo "[INFO] - Memória RAM disponível para o sistema: 
${MEM_LIVRE}MB" >> "$LOG_FILE" 
echo -e "Verificação concluída com sucesso.\n" >> "$LOG_FILE"
