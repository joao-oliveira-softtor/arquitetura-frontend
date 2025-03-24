# Clean Architecture com Next.js e InversifyJS

Este repositório apresenta uma arquitetura frontend baseada na **Clean Architecture** utilizando **Next.js** e **InversifyJS** para injeção de dependências, garantindo modularidade e testabilidade.

## 📂 Estrutura do Projeto
```
📂 src
 ├── 📂 app                # Páginas do Next.js (App Router)
 │    ├── 📂 users
 │    │    ├── 📄 page.tsx  # Página de listagem de usuários
 │    ├── 📄 layout.tsx    # Layout padrão
 ├── 📂 core               # Código central da aplicação (Clean Architecture)
 │    ├── 📂 domain        # Camada de Domínio (regras de neǵocio)
 │    │    ├── 📂 models       # Modelos de Entidades
 │    │    ├── 📂 repositories # Contratos de repositorios
 │    │    ├── 📂 validators   # Casos de Uso
 │    ├── 📂 application     # Camada de Aplicação
 │    │    ├── 📂 interfaces # Interfaces dos repositórios
 │    │    ├── 📂 usecases   # Casos de Uso
 │    │    ├── 📂 services   # Serviços de Aplicação (ViewModels)
 │    ├── 📂 infrastructure  # Camada de Infraestrutura
 │    │    ├── 📂 repositories # Implementações dos Repositórios
 │    ├── 📂 di            # InversifyJS Container
 │    │    ├── 📄 types.ts  # Tipos de dependências
 │    │    ├── 📄 container.ts # Container IoC do InversifyJS
 ├── 📂 hooks              # Hooks personalizados
 ├── 📂 components         # Componentes UI (Atomic Design)
 ├── 📂 styles             # Estilos
 ├── 📂 utils              # Funções auxiliares
 ├── 📂 config             # Configurações globais
 ├── 📄 next.config.js      # Configuração do Next.js
 ├── 📄 package.json        # Dependências
```

## ⚡ Principais Tecnologias
- **Next.js** (App Router)
- **React**
- **TypeScript**
- **InversifyJS** (Injeção de Dependências)
- **Fetch API** (para consumo de dados)

## 🚀 Uso
### 1️⃣ Criando um Hook para a ViewModel
📄 `src/hooks/useUserViewModel.ts`
```tsx
import { container } from "@/core/di/container";
import { TYPES } from "@/core/di/types";
import { UserViewModel } from "@/core/application/services/UserViewModel";

export const useUserViewModel = (): UserViewModel => {
  return container.get<UserViewModel>(TYPES.UserViewModel);
};
```

### 2️⃣ Criando a Página de Usuários no Next.js
📄 `src/app/users/page.tsx`
```tsx
"use client";

import { useEffect, useState } from "react";
import { useUserViewModel } from "@/hooks/useUserViewModel";
import { User } from "@/core/domain/models/User";

export default function UsersPage() {
  const userViewModel = useUserViewModel();
  const [users, setUsers] = useState<User[]>([]);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const loadUsers = async () => {
      try {
        const data = await userViewModel.fetchUsers();
        setUsers(data);
      } catch (err) {
        setError(err instanceof Error ? err.message : "Erro desconhecido");
      }
    };

    loadUsers();
  }, [userViewModel]);

  return (
    <div>
      <h1>Lista de Usuários</h1>
      {error && <p>Erro: {error}</p>}
      <ul>
        {users.map((user) => (
          <li key={user.id}>{user.name} ({user.email})</li>
        ))}
      </ul>
    </div>
  );
}
```

## 🔗 Referências
- [InversifyJS Docs](https://inversify.io/)
- [Next.js Docs](https://nextjs.org/docs)
- [Clean Architecture](https://blog.cleancoder.com/)

Ficou alguma dúvida ou sugestão? Abra uma issue! 🚀

