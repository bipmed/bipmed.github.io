---
layout: page
title: Beacon
subtitle: Light at the end of the tunnel.
---
<div class="container-fluid">
    <div class="row">
    	<form id="searchForm">
          <div class="col-xs-12 col-md-12 form-group">
            <div class="input-group">
              <div class="input-group-btn">
                <select name="assemblyId" class="form-control" style="width: 120px" required>
                    <option value="GRCh38" selected>GRCh38</option>
                    <option value="GRCh37">GRCh37</option>
                </select>
              </div>
              	<input type="text" name="query" value="1 : 785909 C > G" class="form-control" required>
              <div class="input-group-btn">
                <input type="submit" class="btn btn-primary" id="searchButton" value="Search Variant"/>
              </div>
            </div>
          </div>
    	</form>
    </div>
    <div class="row">
        <div class="col-xs-12 col-md-12">
            <table id="result-table" class="table table-bordered" hidden="true">
                <thead>
                    <tr>
                        <th>Dataset</th>
                        <th>Samples</th>
                        <th>Variants</th>
                        <th>Calls</th>
                        <th>Frequency</th>
                    </tr>
                </thead>
                <tbody class="tabletr"></tbody>
            </table>
        </div>
    </div>
    <div style="text-align: right;">
    	<a target="_blank" href="https://github.com/labbcb/beacons/issues/new">Report issue</a>
    </div>
</div>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
<script>
    $(function () {
        $("#searchForm").submit(function (e) {
            e.preventDefault();

            const regex = /^\s*([1-9]|1[0-9]|2[0-2]|[XY])\s*:\s*(\d+)\s*([ACGT]+)\s*>\s*([ACGT]+)$/;
            const array = regex.exec(e.currentTarget.query.value);

            if (array == null || array.length !== 5) {
            	$(".tabletr").html('<tr><td class="alert alert-danger" colspan="5" align="center">Invalid query!</td></tr>');
            	$("#result-table").show();
                return;
            }

            //do your own request an handle the results
            var response = $.get("https://bcbcloud.fcm.unicamp.br/beacons/bipmed/query", {
                assemblyId: e.currentTarget.assemblyId.value,
                referenceName: array[1],
                start: array[2],
                referenceBases: array[3],
                alternateBases: array[4],
                includeDatasetResponses: "HIT"
            });

            response.done(function (result) {
                if (result.exists) {
                    var string = "";
                    for (var i = 0; i <= result.datasetResponses.length - 1; i++) {
                        string += "<tr>";
                        string += "<td>"+result.datasetResponses[i].datasetId+"</td>";
                        string += "<td>"+result.datasetResponses[i].sampleCount+"</td>";
                        string += "<td>"+result.datasetResponses[i].variantCount+"</td>";
                        string += "<td>"+result.datasetResponses[i].callCount+"</td>";
                        string += "<td>"+result.datasetResponses[i].frequency.toFixed(2)+"</td>";
                        string += "</tr>";
                    }
                    $(".tabletr").html(string);
                } else {
                    $(".tabletr").html('<tr><td class="alert alert-warning" colspan="5" align="center">Not found!</td></tr>');
                }
                $("#result-table").show();
            });

            response.fail(function (error) {
                $(".tabletr").html('<tr><td class="alert alert-danger" colspan="5" align="center">Server error!</td></tr>');
                $("#result-table").show();
            })
        });
    });
</script>
