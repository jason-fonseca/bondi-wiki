    var t = new TextStream();

    # set the input encoding
    t.InputEncoding = "iso-8859-1";
    t.Input = encoded_text;

    # this will always return UTF-8
    return t.Read();
