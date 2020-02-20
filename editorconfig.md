# EditorConfig

## Configuration

```sh
cat << EOF > ./.editorconfig
root = true

[*]
charset = utf-8
end_of_line = lf
indent_size = 2
indent_style = space
insert_final_newline = true
max_line_length = 80
trim_trailing_whitespace = true

[*.md]
max_line_length = 0
trim_trailing_whitespace = false
EOF
```

## Tips

### Edit Commit Messages

```conf
[COMMIT_EDITMSG]
max_line_length = 0
```