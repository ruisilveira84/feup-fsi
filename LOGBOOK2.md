# Trabalho realizado nas Semanas #2 e #3

CVE-2022-0787

## Identificação

- A vulnerabilidade no plug-in Limit Login Attempts (Spam Protection) no WordPress refere-se à falha na sanitização e validação inadequada de parâmetros utilizados em instruções SQL através de ações AJAX acessíveis a utilizadores não autenticados, permitindo injeções SQL.

- Aplicações/Sistemas Operativos Relevantes: A vulnerabilidade afeta qualquer website que utilize WordPress com este plug-in numa versão anterior à 5.1.

## Catalogação

- A vulnerabilidade foi descoberta inicialmente por Cydave (Security Engineer) e publicada publicamente em 02/03/2022.

- Foi-lhe atribuído o nível de gravidade 9.8 (crítico).

## Exploit

- Exploits para essa vulnerabilidade envolvem a manipulação de parâmetros enviados através de ações AJAX, visando inserir ou modificar instruções SQL para comprometer a integridade do banco de dados.

- Dada a simplicidade desta vulnerabilidade, não é necessária nem existem ferramentas de automação como módulos de Metasploit dedicados a esta.

## Ataques

- A exploração bem-sucedida dessa vulnerabilidade pode levar a danos significativos, incluindo comprometimento de dados sensíveis, perda de integridade do banco de dados e possíveis violações de privacidade dos utilizadores.
