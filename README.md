# Gpacalculator
<!DOCTYPE html>
<html>
<head>
   <title>GPA Calculator</title>
   <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css">
</head>
<body>
<div id="lcol" class="">
   <div id="doc" class=" p-3 bg-secondary text-white  ">
      <div class="p-2 bg-info text-warning">
         <h1 class="text-warning text-center">GPA Calculator</h1>
         <form id="calcform" name="calcform" class="calc" autocomplete="off">
            <table id="tbl">
               <tr>
                  <td>Class</td>
                  <td>Grade</td>
                  <td>Credits /<br>Hours</td>
               </tr>
               <tbody>
               </tbody>
            </table>
            <div id="cumdiv" class="form-group">
               <label for="gpa">Previous cumulative GPA/credits (optional)</label>
               <div class="row">
                  <div class="col">
                     <input type="number" id="cgpa" min="0" step="any" placeholder="cumulative gpa" class="form-control">
                  </div>
                  <div class="col">
                     <input type="number" id="chours" min="0" step="any" placeholder="credits" class="form-control">
                  </div>
               </div>
            </div>
            <div class="form-group">
               <button type="button" id="calcbtn" title="Calculate" class="btn btn-primary"> Calculate</button>
               <button type="reset" id="resetbtn" title="Reset" class="btn btn-danger"> Reset</button>
               <button type="button" id="addrow" title="Add row" class="btn btn-warning"> Row</button>
            </div>
            <div class="form-group">
               <label for="gpa">GPA</label>
               <input type="text" id="gpa" readonly class="form-control">
            </div>
            <div class="form-group">
               <div id="gpacircle" class="red small mb-3"></div>
            </div>
            <div class="form-group">
               <label for="total">Total credits/hours</label>
               <input type="number" step="any" id="total" class="form-control" readonly>
            </div>
            <div class="form-group">
               <label for="area">GPA calculation</label>
               <input rows="3" id="area" class="form-control" readonly>
            </div>
         </form>
      </div>
   </div>
</div>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
<script>
   var irow=0;
   $( document ).ready(function() {
      $("#calcbtn").click( Calc );
      $("#resetbtn").click( OnReset );
      $("#addrow").click( AddRow );
      for(i=0; i<6; i++)
         AddRow();
   });
   function OnReset()
   {
      $("#gpacircle").hide();
   }
   function Calc()
   {
      var glook=[-1,10.00, 9.00, 8.00, 7.00, 6.00, 0.00];
      var gpa=0;
      var sum=0;
      var txt1="";
      var txt2="";
      for(var i=1; i<=irow+1; i++)
      {
         hours = $("#tbl > tbody > tr:nth-child("+i+") > td:nth-child(3) > input").val();
         hours = parseFloat(hours);
         igrade = $("#tbl > tbody > tr:nth-child("+i+") > td:nth-child(2) > select").prop("selectedIndex");
         grade = glook[igrade];
         if( hours>0 && grade>=0 )
         {
            gpa+=hours*grade;
            sum+=hours;
            txt1+=hours+"\u00D7"+grade+"+";
            txt2+=hours+"+";
         }
      }
      cgpa=$("#cgpa").val();
      chours=$("#chours").val();
      cgpa = parseFloat(cgpa);
      chours = parseFloat(chours);
      if( cgpa>=0 && chours>0 )
      {
         gpa+=chours*cgpa;
         sum+=chours;
         txt1+=chours+"\u00D7"+cgpa+"+";
         txt2+=chours+"+";
      }
      gpa/=sum;
      gpa=gpa.toFixed(2);
      txt1=txt1.slice(0, -1);
      txt2=txt2.slice(0, -1);
      var txt="("+txt1+") / ("+txt2+") = "+gpa;
      $("#gpa").val(gpa);
      $("#total").val(sum);
      $("#area").val(txt);
      var percent=25*gpa;
      if( percent>100 ) percent=100;
      $("#gpacircle").percircle({percent: percent,text: gpa});
      $("#gpacircle").show();
   }
   function AddRow()
   {
      $('#tbl > tbody > tr').eq(irow++).after("<tr>\
         <td><input type='text' name='class[]' class='form-control' placeholder='class "+irow+"'></td>\
         <td><select name='grade[]' class='form-control'>\
            <option selected>--</option>\
            <option>O</option>\
            <option>A+</option>\
            <option>A</option>\
            <option>B+</option>\
            <option>B</option>\
            <option>RA</option>\
         </select></td>\
         <td><input type='number' min='0' step='any' name='hours[]' class='form-control'></td>\
      </tr>");
   }
</script>
</body>
</html>
