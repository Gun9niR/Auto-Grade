# 上海交通大学自动评教脚本(Auto Grade）

# 手把手教你使用评教脚本（中文）
## 第一步
进入你的**学生评价**界面。

## 第二步
复制粘贴如下代码：
```javascript
var ungraded;
var numOfUngraded = $('tr:contains("未评")').length;

function waitForAppearance(element, callBack) {
    if($(".alert-modal").length) {
        return;
    }
    window.setTimeout( function() {
        if($(element).length){
            if(element == 'div.col-sm-8:contains("当前评价课程为")') {
                autoGrade();
            } else {
                waitForDisappearance();
            }
        }else{
            waitForAppearance(element, callBack);
        }
    }, 200);
}

function waitForDisappearance() {
    window.setTimeout( function() {
        if($(".success-modal").length == 0) {
            waitForRefresh();
        } else {
            waitForDisappearance();
        }
    }, 200);
}

function waitForRefresh() {
    window.setTimeout( function() {
        var num = $('tr:contains("未评")').length;
        if(num != numOfUngraded) {
            numOfUngraded = num;
            gotoNext();
        } else {
            waitForRefresh();
        }
    }, 5000);
}

function gotoNext() {
    ungraded = $('tr:contains("未评"):first');
    if(ungraded.length == 0) return;

    $("#tempGrid").setSelection(ungraded.attr("id"));

    waitForAppearance('div.col-sm-8:contains("当前评价课程为")', autoGrade);
}

function autoGrade() {
    $(".input-xspj-1 label input").prop("checked",true);
    let pjContent = $('#jsxm').html() + "老师辛苦了！";
    $("textarea.input-zgpj").text(pjContent);
    $("#btn_xspj_tj").click();
    $('#btn_ok').click();
    waitForRefresh();
}

gotoNext();
```

## 第三步
在你打开评价界面的浏览器，按下**F12**。（mac需要按住**fn**键才会出现F12）

## 第四步
在出现的界面中，找到位于上侧的**Console**，点击进入。把刚才复制的内容**粘贴**进去。再按下**回车键**。

## 第五步
开始自动评教。主观题内容自动填写为 “教师姓名 + 老师辛苦了！”。

## 第六步
完成所有评教。教程结束。


# Illustration(English)
# Auto-Grade
A simple script that comes in two versions, saving you the trouble of having to click all those radios while grading your courses.
## Usage
No need to install anything. Just navigate to "学生评价" webpage, copy one of the versions and paste it in the console of your browser.

## Known issues
- This script only works for the first page of courses, namely the first 15 courses.
- If your Internet connection is too slow, a course may be graded repeatedly. To avoid that, change `5000` in `waitForRefresh()` to a larger number.

## Semi-auto Version
This version won't automatically fill in the "主观题" section: a prompt will pop up asking you to fill in whatever you want to say about the course. And after grading a course, you will have to confirm "提交成功" in order to proceed.

Copy & Paste the code snippet below

```
var ungraded;
var numOfUngraded = $('tr:contains("未评")').length;

function waitForAppearance(element, callBack) {
    if($(".alert-modal").length) {
        return;
    }
    window.setTimeout( function() {
        if($(element).length){
            if(element == 'div.col-sm-8:contains("当前评价课程为")') {
                autoGrade();
            } else {
                waitForDisappearance();
            }
        }else{
            waitForAppearance(element, callBack);
        }
    }, 200);
}

function waitForDisappearance() {
    window.setTimeout( function() {
        if($(".success-modal").length == 0) {
            waitForRefresh();
        } else {
            waitForDisappearance();
        }
    }, 200);
}

function waitForRefresh() {
    window.setTimeout( function() {
        var num = $('tr:contains("未评")').length;
        if(num != numOfUngraded) {
            numOfUngraded = num;
            gotoNext();
        } else {
            waitForRefresh();
        }
    }, 5000);
}

function gotoNext() {
    ungraded = $('tr:contains("未评"):first');
    if(ungraded.length == 0) return;

    $("#tempGrid").setSelection(ungraded.attr("id"));

    waitForAppearance('div.col-sm-8:contains("当前评价课程为")', autoGrade);
}

function autoGrade() {
    $(".input-xspj-1 label input").prop("checked",true);
    let courseInfo = $('div.col-sm-8:contains("当前评价课程为")').text();
    courseInfo = courseInfo.substring(courseInfo.indexOf("当前"), courseInfo.indexOf(";"))
    $("textarea.input-zgpj").text(prompt(courseInfo + "，主观题填什么？"));
    $("#btn_xspj_tj").click();
    waitForAppearance(".success-modal", waitForDisappearance);
}

gotoNext();
```

## Auto version
This version will automatically grade all ungraded courses non-stop, filling in the "主观题" section with pre-defined text. The default text is `教师姓名 + "老师辛苦了！"`. If you want something else, change `pjContent` in `autoGrade()`.

Copy & Paste the code snippet below
```
var ungraded;
var numOfUngraded = $('tr:contains("未评")').length;

function waitForAppearance(element, callBack) {
    if($(".alert-modal").length) {
        return;
    }
    window.setTimeout( function() {
        if($(element).length){
            if(element == 'div.col-sm-8:contains("当前评价课程为")') {
                autoGrade();
            } else {
                waitForDisappearance();
            }
        }else{
            waitForAppearance(element, callBack);
        }
    }, 200);
}

function waitForDisappearance() {
    window.setTimeout( function() {
        if($(".success-modal").length == 0) {
            waitForRefresh();
        } else {
            waitForDisappearance();
        }
    }, 200);
}

function waitForRefresh() {
    window.setTimeout( function() {
        var num = $('tr:contains("未评")').length;
        if(num != numOfUngraded) {
            numOfUngraded = num;
            gotoNext();
        } else {
            waitForRefresh();
        }
    }, 5000);
}

function gotoNext() {
    ungraded = $('tr:contains("未评"):first');
    if(ungraded.length == 0) return;

    $("#tempGrid").setSelection(ungraded.attr("id"));

    waitForAppearance('div.col-sm-8:contains("当前评价课程为")', autoGrade);
}

function autoGrade() {
    $(".input-xspj-1 label input").prop("checked",true);
    let pjContent = $('#jsxm').html() + "老师辛苦了！";
    $("textarea.input-zgpj").text(pjContent);
    $("#btn_xspj_tj").click();
    $('#btn_ok').click();
    waitForRefresh();
}

gotoNext();
```

## Credit
[@xx01cyx](https://github.com/xx01cyx) came up with the idea and implemented the auto version. She's AWESOME.
