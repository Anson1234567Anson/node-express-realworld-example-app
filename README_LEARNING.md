# RealWorld 全栈学习项目（后端：Node + Express + Prisma + PostgreSQL）

配套前端：https://github.com/Anson1234567Anson/react-redux-realworld-example-app

## 本地运行
```bash
# 1. 准备 PostgreSQL（Docker 或本机安装都行）
docker run -d --name rw-pg -e POSTGRES_USER=rw -e POSTGRES_PASSWORD=rw -e POSTGRES_DB=realworld -p 5432:5432 postgres:16
# 2. 环境变量（.env 已在 .gitignore 中）
cat > .env <<EOF
DATABASE_URL=postgresql://rw:rw@localhost:5432/realworld
JWT_SECRET=dev-secret
NODE_ENV=development
EOF
# 3. 安装、迁移、填充假数据
npm install
npx prisma migrate deploy
npx prisma db seed
# 4. 启动（默认端口 3000，可用 PORT=3300 改）
npx nx serve
# 5. 验证
curl http://localhost:3000/api/tags
```

## 目录地图
- `src/main.ts`：Express 入口（cors、body-parser、路由、错误处理）
- `src/app/routes/routes.ts`：汇总所有路由
- `src/app/routes/auth/`：注册/登录/当前用户（bcrypt 加密、JWT 签发，`express-jwt` 校验）
- `src/app/routes/article/`：文章增删改查、分页、收藏、评论（controller → service → Prisma）
- `src/app/routes/profile/`、`tag/`：用户主页、关注、标签
- `src/prisma/schema.prisma`：数据模型（User、Article、Comment、Tag 及多对多关系）
- `src/prisma/migrations/`、`seed.ts`：迁移与假数据
- `e2e/`、`src/tests/`：测试

## 建议阅读路线
1. 注册：`auth.controller.ts` → `auth.service.ts` → `prisma.user.create`
2. 发文章：`article.controller.ts` → `article.service.ts`（slug 生成、标签 connectOrCreate）
3. 练手：给 Article 加 `viewsCount` 字段 → 改 schema → `npx prisma migrate dev` → 在 GET 文章时 +1
