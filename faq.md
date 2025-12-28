## ClassInsights API Server IP ändern

Die IP-Addresse des lokalen ClassInsights Servers hat sich geändert? Damit die Computer wissen wohin sie sich nun verbinden müssen, müssen Sie ihnen die neue IP geben.
Dies lässt sich wiefolgt ganz einfach machen:

Führen Sie einfach folgenden Befehl auf Ihrem **Domain Controller** aus:

```
Set-GPRegistryValue -Name "ClassInsights" -Key "HKLM\SOFTWARE\ClassInsights" -Type String -ValueName "ApiUrl" -Value "https://<IHRE-NEUE-SERVER-IP>:52001"
```

> Vergessen Sie nicht den Platzhalter mit ihrer IP am Ende des Befehls zu ersetzen!

Nun müssen Sie nur noch bei https://classinsights.at/schulen unter dem Einstellungssymbol die URLs mit der neuen IP aktualisieren.
