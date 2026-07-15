# 🤿 Dive Logbook

Logbook de mergulho pessoal — app HTML autónoma, sem dependências de build.

## Funcionalidades
- Registo completo: perfil do mergulho, equipamento (fato/garrafa/pesos), notas e vida marinha, rating em bolhas
- Importação do computador de mergulho (CSV, Subsurface XML, UDDF) com deteção de duplicados, preview e undo
- Fotos da página do logbook de papel e do carimbo, com recriação digital do carimbo por IA*
- Pré-preenchimento do formulário a partir da foto do logbook por IA*
- Mapa dos sítios de mergulho (Leaflet + OpenStreetMap, geocodificação Nominatim)
- Cópia de segurança em JSON
- Contas com link mágico (Supabase) e sincronização automática entre dispositivos
- Funciona offline; sincroniza quando há rede
- Dados guardados localmente no browser (localStorage)

\* As funções de IA requerem execução no preview do Claude.

## Usar
Abrir `index.html` no browser, ou ativar GitHub Pages (Settings → Pages → Deploy from branch → main) e aceder pelo URL publicado.

## Nota sobre dados
Os registos vivem no localStorage de **cada dispositivo** — telemóvel e portátil não sincronizam entre si. Usa "Guardar cópia de segurança" para exportar/migrar.
