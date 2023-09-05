# learning<?php
session_start();
if(!$_SESSION['username'])
{
    header('Location:login');
    session_destroy();
}
include 'includes/__dbconnect.php';
require 'vendor/autoload.php';
$sel2=mysqli_query($conn01,"select user_id  from userlogin where username='".$_SESSION['username']."'");
$row2=mysqli_fetch_array($sel2);
$c=mysqli_query($conn01,"select * from courses where courseid =".$_GET['courseid']." && createdby=".$row2['user_id']);
if(mysqli_num_rows($c)>0)
{
include 'includes/__header.php';
include 'includes/__adminnavbar.php';
include 'includes/__admintopnav.php';
?>
<!-- ============================================================== -->
            <!-- Start right Content here -->
            <!-- ============================================================== -->
            <div class="main-content">

                <div class="page-content">
                    <div class="container-fluid">

                        <!-- start page title -->
                        <div class="row">
                            <div class="col-12">
                                <div class="page-title-box d-sm-flex align-items-center justify-content-between">
                                    <h4 class="mb-sm-0 font-size-18">Chapters</h4>

                                    <div class="page-title-right">
                                        <ol class="breadcrumb m-0">
                                            <li class="breadcrumb-item"><a href="admindashboard">Home</a></li>
                                            <li class="breadcrumb-item active">Chapters</li>
                                        </ol>
                                    </div>

                                </div>
                            </div>
                        </div>
                        <!-- end page title -->
                                        <?php
                                        $course=mysqli_query($conn01,"select * from courses where courseid=".$_GET['courseid']);
                                        $coursedetails=mysqli_fetch_array($course);
                                        ?>
                        <div class="row">
                            <div class="col-12">
                                <div class="card">
                                    <div class="card-header">
                                    <button type="button" class="btn btn-danger w-lg waves-effect waves-light add_btn" data-bs-toggle="modal" data-bs-target="#AddCourse" >Add Chapter</button>


                        <!-- The Modal -->
                        <div class="modal" id="AddCourse">
                        <div class="modal-dialog modal-dialog-centered">
                            <div class="modal-content">

                                <!-- Modal Header -->
                                <div class="modal-header">
                                    <h4 class="modal-title">Add Chapter</h4>
                                    <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
                                </div>

                                <!-- Modal body -->
                                <div class="modal-body" id="add_admins">
                                <form  method="POST" class="w-100" enctype="multipart/form-data">
                                            <div class="col-lg-12">
                                                <div>
                                                    <div class="mb-3">
                                                        <label for="example-text-input" class="form-label">Course Name</label>
                                                        <input class="form-control" type="text" name="course_name" readonly value ="<?php echo $coursedetails['course_name']?>" placeholder="Enter Course Name" id="example-text-input">
                                                    </div>
                                                    
                                                </div>
                                            </div>
                                            <div class="col-lg-12">
                                                <div>
                                                    <div class="mb-3">
                                                        <label for="example-text-input" class="form-label">Chapter Name</label>
                                                        <input class="form-control" type="text" name="chaptername" placeholder="Enter Chapter Name" id="example-text-input">
                                                    </div>
                                                    
                                                </div>
                                            </div>
                                            <div class="col-lg-12">
                                                                <div class="mb-3">
                                                                    <label for="basicpill-address-input" class="form-label">Chapter Summary</label>
                                                                    <textarea id="basicpill-address-input" name="chaptersummary" class="form-control" rows="2" placeholder="Enter Summary"></textarea>
                                                                </div>
                                                            </div>
                                                            <div class="mb-3">
                                            <input type="file" name="videofile" id="video" class="form-control" title = "Upload video file">
                                            <p class="help-block" style="color:#900;">Upload video file</p>
                                        </div>
					                    <div class="mb-3">
                                            <input type="file" name="pdffile" id="pdf" class="form-control" title = "Upload pdf file">
                                            <p class="help-block" style="color:#900;">Upload pdf file</p>
                                        </div>
                                        <div class="mb-3">
                                            <input type="file" name="pptxfile" id="pptx" class="form-control" title = "Upload pptx file">
                                            <p class="help-block" style="color:#900;">Upload pptx file</p>
                                        </div>
                                        <!-- <button type="submit" class="btn btn-primary">Submit</button> -->
                                    
                                    
                                </div>

                                <!-- Modal footer -->
                                <div class="modal-footer ">
                                    <button type="submit" name="addchapter" id="addadmin_btn"class="btn btn-success ">Add</button>
                                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                                </div>
                                </form>
                            </div>
                        </div>
                    </div>


                                        <!-- <h4 class="card-title">Default Datatable</h4>
                                        <p class="card-title-desc">DataTables has most features enabled by
                                            default, so all you need to do to use it with your own tables is to call
                                            the construction function: <code>$().DataTable();</code>.
                                        </p> -->
                                    </div>
                                <!--########################### ADD OUTPUT########################### -->
                                <div class="outputadd" id="outputadd">
                                    
                                    </div>

 

                                <!--########################### ADD OUTPUT########################### -->

                                    <div class="card-bod table-responsive">
                                        <?php
                                        $sel2=mysqli_query($conn01,"select user_id  from userlogin where username='".$_SESSION['username']."'");
                                        $row2=mysqli_fetch_array($sel2);
                                          $query_run="select courses.*,coursedetails.* from courses,coursedetails where courses.courseid =coursedetails.courseid && coursedetails.courseid=".$_GET['courseid']." && coursedetails.createdby=".$row2['user_id']."";
                                          $sql_query_run=mysqli_query($conn01,$query_run);
                                        ?>
        
                                        <table id="datatable" class="table table-bordered nowrap w-100">
                                            <thead>
                                            <tr>
                                                <th>#</th>
                                                <th>Chapter Name</th>
                                                <th>Course Name</th>
                                                <th>Chapter Summary</th>
                                                <th>Video</th>
                                                <th>PDF</th>
                                                <th>PPTX</th>
                                                <th class="sort_del1">Action</th>
                                                <th class="sort_del2"></th>
                                            </tr>
                                            </thead>
        
        
                                            <tbody>
                                               <?php
                                               if(mysqli_num_rows($sql_query_run) > 0)
                                               {
                                                    $i=1;
                                                   while($rownum = mysqli_fetch_assoc($sql_query_run))
                                                   {
                                                       $ext = explode(".",$rownum['videopath'])
                                               ?>
                                            <tr>
                                                <td><?php echo $i; ?></td>
                                                <td><?php echo $rownum['chaptername']; ?></td>
                                                <td><?php echo $rownum['course_name']; ?></td>
                                                <td><?php echo substr($rownum['chaptersummary'],0,50); ?>...</td>
                                                <td><?php echo "<video width='150' height='100' controls>
                                                                            <source src='".$rownum['videopath']."' type='video/".$ext[1]."'>
                                                                            </video>"; ?></td>
                                                 <td><a href="pdf/<?php echo $rownum['pdfpath']; ?>" target="_blank"><img src="assets/images/pdf.png" style="width:30px; height:35px;"/></a></td>
                                                 <td><a href="pptx/<?php echo $rownum['pptxpath']; ?>" target="_blank"><img src="assets/images/pptx.png" style="width:30px; height:35px;"/></a></td>
                                                <td >
                                            <div class="d-flex justify-content-evenly flex-wrap gap-2">
                                                
                                                
                                            <button class="btn btn-success waves-effect waves-light edit_btn" data-bs-toggle="modal" data-bs-target="#ChapterEdit<?php echo $rownum['chapter_id']; ?>">
                                            <i class="mdi mdi-pencil  font-size-18 align-middle me-2"></i>Edit</button>
                                            
                                            </div>
                                            <!-- The Modal -->
                                                <div class="modal" id="ChapterEdit<?php echo $rownum['chapter_id']; ?>">
                                                <div class="modal-dialog modal-dialog-centered">
                                                    <div class="modal-content">

                                                        <!-- Modal Header -->
                                                        <div class="modal-header">
                                                            <h4 class="modal-title">Edit Chapter</h4>
                                                            <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
                                                        </div>

                                                        <!-- Modal body -->
                                                        <div class="modal-body" id="add_admins">
                                                    <form  method="POST" class="w-100" enctype="multipart/form-data">
                                                    <div class="col-lg-12">
                                                <div>
                                                    <div class="mb-3">
                                                        <label for="example-text-input" class="form-label">Course Name</label>
                                                        <input class="form-control" type="text" name="course_name" readonly value ="<?php echo $coursedetails['course_name']?>" placeholder="Enter Course Name" id="example-text-input">
                                                    </div>
                                                    
                                                </div>
                                            </div>
                                            <div class="col-lg-12">
                                                <div>
                                                    <div class="mb-3">
                                                        <label for="example-text-input" class="form-label">Chapter Name</label>
                                                        <input class="form-control" type="text" name="chaptername" value="<?php echo $rownum['chaptername']; ?>" placeholder="Enter Chapter Name" id="example-text-input">
                                                    </div>
                                                    
                                                </div>
                                            </div>
                                            <div class="col-lg-12">
                                                                <div class="mb-3">
                                                                    <label for="basicpill-address-input" class="form-label">Chapter Summary</label>
                                                                    <textarea id="basicpill-address-input" name="chaptersummary" class="form-control" rows="2" placeholder="Enter Summary"><?php echo $rownum['chaptersummary']; ?></textarea>
                                                                </div>
                                                            </div>
                                                                <div class="mb-3">
                                                                    <input type="file" name="videofile" id="video<?php echo $rownum['chapter_id']; ?>" class="form-control" onchange='readURL(this);' title = "Upload video file">
                                                                    <p class="help-block" style="color:#900;">Upload video file</p>
                                                                    <video width='150' height='100' controls  id="userimg">
                                                                            <source src='<?php echo $rownum['videopath'] ?>' type='video/<?php echo $ext[1] ?>'>
                                                                            </video>
                                                                            <script>
                                                                                function readURL(input) {
                                                                                        if (input.files && input.files[0]) {
                                                                                            var reader = new FileReader();

                                                                                            reader.onload = function (e) {
                                                                                                $('#userimg source')
                                                                                                    .attr('src', e.target.result)
                                                                                                    .width(150)
                                                                                                    .height(100);
                                                                                            };

                                                                                            reader.readAsDataURL(input.files[0]);
                                                                                        }
                                                                                    }
                                                                            </script>
                                                                </div>
                                                                <div class="mb-3">
                                                                    <input type="file" name="pdffile" id="pdf<?php echo $rownum['chapter_id']; ?>" class="form-control" onchange='readURL1(this);' title = "Upload pdf file">
                                                                    <p class="help-block" style="color:#900;">Upload pdf file</p>
                                                                    <a href="pdf/<?php echo $rownum['pdfpath']; ?>" id="userpdf" target="_blank"><img src='assets/images/pdf.png' style="width:30px; height:35px;"/></a>
                                                                            <script>
                                                                                function readURL1(input) {
                                                                                        if (input.files && input.files[0]) {
                                                                                            var reader = new FileReader();

                                                                                            reader.onload = function (e) {
                                                                                                
                                                                                                $('#userpdf')
                                                                                                    .attr('href', e.target.result)
                                                                                                    .width(50)
                                                                                                    .height(50);
                                                                                            };

                                                                                            reader.readAsDataURL(input.files[0]);
                                                                                        }
                                                                                    }
                                                                            </script>
                                                                </div>
                                                                <div class="mb-3">
                                                                    <input type="file" name="pptxfile" id="pptx<?php echo $rownum['chapter_id']; ?>" class="form-control" onchange='readURL2(this);' title = "Upload pptx file">
                                                                    <p class="help-block" style="color:#900;">Upload pptx file</p>
                                                                    <a href="pptx/<?php echo $rownum['pptxpath']; ?>" id="userpptx" target="_blank"><img src='assets/images/pptx.png' style="width:30px; height:35px;"/></a>
                                                                            <script>
                                                                                function readURL2(input) {
                                                                                        if (input.files && input.files[0]) {
                                                                                            var reader = new FileReader();

                                                                                            reader.onload = function (e) {
                                                                                                
                                                                                                $('#userpptx')
                                                                                                    .attr('href', e.target.result)
                                                                                                    .width(50)
                                                                                                    .height(50);
                                                                                            };

                                                                                            reader.readAsDataURL(input.files[0]);
                                                                                        }
                                                                                    }
                                                                            </script>
                                                                </div>
                                                                <!-- <button type="submit" class="btn btn-primary">Submit</button> -->
                                                            
                                                            
                                                        </div>

                                                        <!-- Modal footer -->
                                                        <div class="modal-footer ">
                                                            <button type="submit" name="editchapter<?php echo $rownum['chapter_id']; ?>" id="addadmin_btn"class="btn btn-success ">Update</button>
                                                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                                                        </div>
                                                        </form>
                                                    </div>
                                                </div>
                                            </div>
                                        
                                            <?php
                                        if(isset($_POST['editchapter'.$rownum['chapter_id']]))
                                        {
                                            $courseid=$_GET['courseid'];
                                            $chaptername=$_POST['chaptername'];
                                            $chaptersummary=mysqli_real_escape_string($conn01,$_POST['chaptersummary']);
                                            $extension1=explode("/",$_FILES['videofile']['type']);
                                                if($_FILES['videofile']['type']!=null)
                                                {
                                                $videofile=$rownum['chapter_id'].".".$extension1[1];
                                                $s3 = new Aws\S3\S3Client([
                                                    'region'  => 'us-east-2',
                                                    'version' => 'latest',
                                                    'credentials' => [
                                                        'key'    => "AKIAVMHR7ZJ7YSUYP7FP",
                                                        'secret' => "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",
                                                    ]
                                                ]);	
                                                $result = $s3->putObject([
                                                    'Bucket' => 'test-hcfire',
                                                    'Key'    => $videofile,
                                                    'SourceFile' => $_FILES['videofile']['tmp_name']			
                                                ]);
                                                if($result)
                                                {
                                                $videourl = $videofile;
                                                }
                                                }
                                                else if($_FILES['videofile']['type']==null)
                                                {
                                                    $videourl=$rownum['videopath'];
                                                }
                                                $extension2=explode("/",$_FILES['pdffile']['type']);
                                                if($_FILES['pdffile']['type']!=null)
                                                {
                                                $pdffile=$rownum['chapter_id'].".".$extension2[1];
                                                move_uploaded_file($_FILES['pdffile']['tmp_name'],'pdf/'.$pdffile);
                                                }
                                                else if($_FILES['pdffile']['type']==null)
                                                {
                                                    $pdffile=$rownum['pdfpath'];
                                                }
                                                $extension3="pptx";
                                                if($_FILES['pptxfile']['type']!=null)
                                                {
                                                    $pptxfile=$rownum['chapter_id'].".".$extension3;
                                                    move_uploaded_file($_FILES['pptxfile']['tmp_name'],'pptx/'.$pptxfile);
                                                }
                                                else if($_FILES['pptxfile']['type']==null)
                                                {
                                                    $pptxfile=$rownum['pptxpath'];
                                                }
                                            $upd =mysqli_query($conn01,"update coursedetails set courseid=".$courseid.",chaptername='".$chaptername."',chaptersummary='".$chaptersummary."',videopath='".$videourl."',pdfpath='".$pdffile."',pptxpath='".$pptxfile."' where createdby=".$row2['user_id']." && chapter_id=".$rownum['chapter_id']);
                                            if($upd)
                                            {
                                                echo "<script>
                                                alert('Chapter updated successfully');
                                                document.location='add_chapter-".$_GET['courseid']."';
                                                </script>";
                                            }
                                            else
                                            {
                                                echo "<script>
                                                alert('Database Error');
                                                document.location='add_chapter-".$_GET['courseid']."';
                                                </script>";
                                            }
                                        }
                                        ?>
                                        
                                        
                                        </td>
                                                <td><div class="d-flex justify-content-evenly flex-wrap gap-2 ">
                                            <button type="button" class="btn btn-danger waves-effect waves-light" data-bs-toggle="modal" data-bs-target="#ChapterDelete<?php echo $rownum['chapter_id']; ?>">
                                            <i class="mdi mdi-trash-can  font-size-18 align-middle me-2"></i> Delete
                                            </button></div>
                                        
                                        <!-- The Modal -->
                                        <div class="modal" id="ChapterDelete<?php echo $rownum['chapter_id']; ?>">
                                                <div class="modal-dialog modal-dialog-centered">
                                                    <div class="modal-content">

                                                        <!-- Modal Header -->
                                                        <div class="modal-header">
                                                            <h4 class="modal-title">Delete Chapter</h4>
                                                            <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
                                                        </div>

                                                        <!-- Modal body -->
                                                        <div class="modal-body">
                                                            
                                                        <p><div class="alert alert-warning">Are you sure want to delete this Chapter?</p>
                                                        </div>

                                                        <!-- Modal footer -->
                                                        <div class="modal-footer ">
                                                        <form method="post">
                                                        <input type="hidden" name="h1" value="<?php echo $rownum['chapter_id']?>"/>
                                                            <button type="submit" name="deletechapter<?php echo $rownum['chapter_id']; ?>" id="addadmin_btn"class="btn btn-success ">Yes</button>
                                                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">No</button>
                                                        </form>
                                                        </div>
                                                    </div>
                                                </div>
                                            </div>
                                        
                                        
                                            <?php
                                                if(isset($_POST['deletechapter'.$rownum['chapter_id']]))
                                                {
                                                    $chapter_id =$_POST['h1'];
                                        $del=mysqli_query($conn01,"delete from coursedetails where chapter_id =".$chapter_id."");
                                            if($del)
                                                    {
                                                        $bucket = 'test-hcfire';
                                                        $keyname = $rownum['videopath'];

                                                        $s3 = new Aws\S3\S3Client([
                                                            'region'  => 'us-east-2',
                                                            'version' => 'latest',
                                                            'credentials' => [
                                                                'key'    => "AKIAVMHR7ZJ7YSUYP7FP",
                                                                'secret' => "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",
                                                            ]
                                                        ]);	
                                                        $result = $s3->deleteObject([
                                                            'Bucket' => $bucket,
                                                            'Key'    => $keyname
                                                        ]);
                                                        unlink("pdf/".$rownum['pdfpath']);
                                                        unlink("pptx/".$rownum['pptxpath']);
                                                        echo "<script>
                                                                document.location='add_chapter-".$_GET['courseid']."';					
                                                              </script>";
                                                        
                                                    }
                                                }
  ?>
                                 
                                        
                                        </td>
                                            </tr>
                                        
                                            <?php
                                            $i=$i+1;
                                                   }
                                                }
                                            ?>
                                            
                                            </tbody>
                                        </table>
        
                                    </div>
                                </div>
                            </div> <!-- end col -->
                        </div> <!-- end row -->
        
                        
                    </div> <!-- container-fluid -->
                </div>
                <!-- End Page-content -->
                <style>
                    th.sorting.sort_del1{
                          border-right: none !important;
                    }
                    th.sorting.sort_del2{
                        border-left: none !important;
                    }
                    th.sorting.sort_del1::after{
                            display: none !important;
                    }
                    th.sorting.sort_del1::before{
                            display: none !important;
                    }
                    th.sorting.sort_del2::after{
                            display: none !important;
                    }
                    th.sorting.sort_del2::before{
                            display: none !important;
                    }
                </style>
            

   
                
            <?php

//include 'includes/__adminmodal.php';
include 'includes/__settings.php';
include 'includes/__scripts.php';
?>



<?php
//include 'includes/__adminajax.php';
include 'includes/__footer.php';
?>
<?php
if(isset($_POST['addchapter']))
			{
                $courseid=$_GET['courseid'];
                $chaptername=$_POST['chaptername'];
                $chaptersummary=mysqli_real_escape_string($conn01,$_POST['chaptersummary']);
                $ins =mysqli_query($conn01,"insert into coursedetails(courseid,chaptername,chaptersummary,createdby)values(".$courseid.",'".$chaptername."','".$chaptersummary."',".$row2['user_id'].")");
                if($ins)
				{
                    $sel1=mysqli_query($conn01,"select chapter_id from coursedetails where courseid=".$courseid."  &&  chaptername='".$chaptername."' && createdby=".$row2['user_id']." limit 1");
					$row1=mysqli_fetch_array($sel1);
                    $extension1=explode("/",$_FILES['videofile']['type']);
					$videofile=$row1['chapter_id'].".".$extension1[1];
                    $extension2=explode("/",$_FILES['pdffile']['type']);
					$pdffile=$row1['chapter_id'].".".$extension2[1];
                    $extension3="pptx";
					$pptxfile=$row1['chapter_id'].".".$extension3;
                    //move_uploaded_file($_FILES['videofile']['tmp_name'],'Videos/'.$videofile);
                    move_uploaded_file($_FILES['pdffile']['tmp_name'],'pdf/'.$pdffile);
                    move_uploaded_file($_FILES['pptxfile']['tmp_name'],'pptx/'.$pptxfile);
                        $s3 = new Aws\S3\S3Client([
                            'region'  => 'us-east-2',
                            'version' => 'latest',
                            'credentials' => [
                                'key'    => "AKIAVMHR7ZJ7YSUYP7FP",
                                'secret' => "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",
                            ]
                        ]);	
                        $result = $s3->putObject([
                            'Bucket' => 'test-hcfire',
                            'Key'    => $videofile,
                            'SourceFile' => $_FILES['videofile']['tmp_name']			
                        ]);
                        if($result)
		                {
                            $videourl = $videofile;
                            mysqli_query($conn01,"update coursedetails set videopath='".$videourl."' , pdfpath='".$pdffile."',pptxpath='".$pptxfile."' where chapter_id=".$row1['chapter_id']."");
                            
                            echo "<script>
                            alert('Chapter added successfully');
                            document.location='add_chapter-".$_GET['courseid']."';
                            </script>";
                        }
            }
                else
				{
					echo "<script>
					alert('Database Error');
					document.location='add_chapter-".$_GET['courseid']."';
					</script>";
				}
                
            }
            ?>
            <?php
            }
            else
            {
                header('Location:error404');
            }
            ?>
