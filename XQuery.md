# XQuery Notes

## List files in a collection modified after a certain date

    let $fullCollection := xmldb:xcollection("/db/datasets/myapp/data")
    let $filteredCollection := xmldb:find-last-modfiied-since($fullCollection, xs:dateTime("2021-11-23T21:13:00"))
    return $filteredCollection
