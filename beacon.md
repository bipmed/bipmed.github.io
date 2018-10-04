---
layout: page
title: Beacon
subtitle: Light at the end of the tunnel.
---

<div>
    <form id="searchForm" action="https://bcbcloud.fcm.unicamp.br/beacons/bipmed/query">
        <div class="input-group mb-3">
            <div class="input-group-prepend">
                <select name="assemblyId" style="width: 150px" class="custom-select" required>
                    <option value="GRCh38" selected>GRCh38</option>
                    <option value="GRCh37">GRCh37</option>
                </select>
            </div>
            <input type="text" name="query" value="1 : 785909 C > G" class="form-control" required>
            <div class="input-group-append">
                <input type="submit" class="btn btn-primary" id="searchButton" value="Search Variant"/>
            </div>
            <div class="input-group-append">
                <span id="queryResult" style="width: 120px"></span>
            </div>
        </div>
    </form>
</div>
<script
        src="https://code.jquery.com/jquery-3.3.1.min.js"
        integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8="
        crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js"
        integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49"
        crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js"
        integrity="sha384-ChfqqxuZUCnJSK3+MXmPNIyE6ZbWh2IMqE241rYiqJxyMiZ6OW/JmZQ5stwEULTy"
        crossorigin="anonymous"></script>
<script>
    $(function () {
        $("#searchForm").submit(function (e) {
            e.preventDefault();

            const regex = /^\s*([1-9]|1[0-9]|2[0-2]|[XY])\s*:\s*(\d+)\s*([ACGT]+)\s*>\s*([ACGT]+)$/;
            const array = regex.exec(e.currentTarget.query.value);

            if (array == null || array.length !== 5) {
                $("#queryResult").text("Invalid query!");
                $("#queryResult").attr("class", "input-group-text alert-danger");
                return;
            }

            //do your own request an handle the results
            var response = $.get(e.currentTarget.action, {
                assemblyId: e.currentTarget.assemblyId.value,
                referenceName: array[1],
                start: array[2],
                referenceBases: array[3],
                alternateBases: array[4]
            });

            response.done(function (result) {
                if (result.exists) {
                    $("#queryResult").text("Found!");
                    $("#queryResult").attr("class", "input-group-text alert-success");
                } else {
                    $("#queryResult").text("Not found!");
                    $("#queryResult").attr("class", "input-group-text alert-warning");
                }
            });

            response.fail(function (error) {
                console.log("error");
                console.log(error)
            })

        });

    });
</script>
