---
name: git-worktrees
description: 'Manage git worktrees'
---

# Skill: Git Worktree Manager

Esta skill administra el ciclo de vida de Git Worktrees permitiendo crear, mergear y eliminar entornos de trabajo aislados a partir de un identificador de worktree (`worktree-id`).

---

## 📋 Comandos del Workflow

### 1. Crear Worktree
Crea un nuevo directorio de trabajo aislado en `./.worktrees/{worktree-id}` y asigna una nueva rama llamada `feat/{worktree-id}`:

```bash
git worktree add -b feat/{worktree-id} ./.worktrees/{worktree-id}
```

**Ejemplo:**
```bash
git worktree add -b feat/login-screen ./.worktrees/login-screen
```

---

### 2. Mergear Worktree
Fusiona los cambios realizados en la rama `feat/{worktree-id}` dentro de la rama actual (por ejemplo, `main` o `develop`):

```bash
git merge feat/{worktree-id}
```

**Ejemplo:**
```bash
git merge feat/login-screen
```

---

### 3. Eliminar Worktree y Rama
Limpia la estructura de directorios borrando el worktree y posteriormente elimina la rama de Git asociada:

```bash
git worktree remove ./.worktrees/{worktree-id}
git branch -d feat/{worktree-id}
```

**Ejemplo:**
```bash
git worktree remove ./.worktrees/login-screen
git branch -d feat/login-screen
```

---

## ⚙️ Especificaciones de la Skill

- **Nombre de la skill:** `git-worktrees`
- **Prefijo de rama:** `feat/`
- **Directorio base:** `./.worktrees/`
- **Uso previsto:** Automatización de flujos con Git Worktrees en proyectos de desarrollo.