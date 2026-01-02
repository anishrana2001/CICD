### Correct the `index.html` file.

```
cat <<EOF > index.html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Project_1: My First Web Page</title>
</head>
<body>
    <h1>Welcome to My Website under Project_1</h1>
    <p>This is my first paragraph on a basic HTML page.</p>
    <p style="color: red; font-size: 1.5em;">Test Line2 for Project_1</p>
</body>
</html>
EOF
```

```
git add index.html ; git commit -m "Corrected the Index.html file" ; git push origin main
```

### Wait and watch the pipeline. It should work fine.

