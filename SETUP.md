# SETUP — Guia de instalação (admin)

Para quem vai publicar a app. Os utilizadores finais só precisam do COMO-USAR.md.

## 1. Criar o projeto Supabase
1. supabase.com → New project (plano free chega)
2. **Authentication → Sign In / Up:** confirma que "Email" está ativo (magic link / OTP vem ativo por defeito)
3. **Authentication → URL Configuration:** em *Site URL* coloca o URL final da app (ex.: `https://<user>.github.io/dive-logbook/`)

## 2. Criar a tabela (SQL Editor → New query → Run)
```sql
create table public.dives (
  user_id uuid not null references auth.users(id) on delete cascade,
  id text not null,
  payload jsonb not null,
  updated_at timestamptz not null default now(),
  primary key (user_id, id)
);

alter table public.dives enable row level security;

create policy "select own" on public.dives for select using (auth.uid() = user_id);
create policy "insert own" on public.dives for insert with check (auth.uid() = user_id);
create policy "update own" on public.dives for update using (auth.uid() = user_id);
create policy "delete own" on public.dives for delete using (auth.uid() = user_id);
```
As policies de RLS garantem que cada utilizador só lê/escreve os próprios mergulhos.

## 3. Ligar a app
1. Supabase → Settings → API: copia o **Project URL** e a **anon public key**
2. No `index.html`, procura por `SUPABASE_URL` e `SUPABASE_KEY` (perto de "Nuvem (Supabase)") e cola os valores
   - A anon key é pública por natureza — a segurança vem das policies de RLS, não do segredo da chave
3. Commit ao repo

## 4. Publicar (GitHub Pages)
Repo → Settings → Pages → Deploy from a branch → `main` → Save.
O URL fica `https://<user>.github.io/<repo>/`. Confirma que é o mesmo que puseste no passo 1.3.

## Notas
- Sem `SUPABASE_URL`/`SUPABASE_KEY` preenchidos, a app funciona em modo local (sem contas) — útil para testar.
- Limites do free tier do Supabase (500 MB DB) chegam para milhares de mergulhos com fotos comprimidas.
- As funções ✨ de IA (ler carimbo/logbook) só funcionam quando a app corre no preview do Claude — no site publicado ficam desativadas com aviso.
