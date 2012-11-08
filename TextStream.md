    # pass in the encoded text and set the code page
    var t = new TextStream(encodedtext, "iso-8859-1");

    # this will always return UTF-8
    return t.Read();