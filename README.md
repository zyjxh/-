# -
前端小白，学习笔记
多指教！

var reg = /[0-9]+.?[0-9]*/g

var result = "1840015000574720 / 1845355911059968 = 1845356567091712".replace(reg , function(word){
    console.log(word);
    debugger
    return "#"+word+"#"
});
    console.log(result)
    console.log(reg.test("1840015000574720"))