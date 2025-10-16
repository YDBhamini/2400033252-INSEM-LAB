#!/bin/bash

set -e

echo "=== Initialize Git Repository ==="
git init

echo "function greet() { return 'Hello'; }" > App.js
git add App.js
git commit -m "Initial commit with greet() function"

echo "=== Create feature branches ==="
git branch feature1
git branch feature2
echo "=== Alice working on feature1 ==="
git checkout feature1
cat > App.js <<'EOF'
function greet() {
  return "Hello from Alice!";
}
EOF
git add App.js
git commit -m "Alice: update greet message"

echo "=== Bob working on feature2 ==="
git checkout feature2
cat > App.js <<'EOF'
function greet() {
  return "Hello from Bob!";
}
EOF
git add App.js
git commit -m "Bob: update greet message"

echo "=== Merge feature1 into main ==="
git checkout main
git merge feature1 -m "Merge feature1 into main"

echo "=== Merge feature2 into main (expect conflict) ==="
set +e
git merge feature2
set -e

echo
echo "  Merge conflict detected in App.js!"
echo
cat App.js

echo
echo "=== Resolving conflict manually ==="
cat > App.js <<'EOF'
function greet() {
  return "Hello from Alice and Bob!";
}
EOF

git add App.js
git commit -m "Resolve merge conflict between feature1 and feature2"

echo
echo " Merge conflict resolved!"
echo
git log --oneline --graph --all
