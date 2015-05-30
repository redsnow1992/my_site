`let`与`lamba`的统一
下面的表达是相同的：
`(let ((var exp2) exp1))    <=>   ((lambda (var) exp1) exp2)  `